<template>
    <div>
        <input v-if="!sheet" type="file" @change="handleFileChange">
        <div v-if="sheet && cortegeList.length > 0">
            <at-card style="width:300px; display:inline-block; margin-right:8px;margin-bottom:4px;" v-for="(cortege, index) in cortegeList" :key="index">
                <span slot="title" v-if="cortege.title">{{ cortege.title }}</span>
                <at-button-group slot="extra" size="small">
                    <at-button icon="icon-edit" hollow></at-button>
                    <at-button icon="icon-star" click="fave" hollow></at-button>
                    <at-button icon="icon-trash-2" type="error" @click="remove" hollow :value="index"></at-button>
                </at-button-group>
                <at-button @click="fill" :value="JSON.stringify(cortege.value)">
                    {{ cortege.value.join(" ") }}
                </at-button>
            </at-card>
        </div>
        <div v-if="sheet">
        <table style="max-width: 860px;">
            <tbody>
            <tr>
                <td style="font-weight:700">{{ sheet.C5.v }}</td>
                <td></td>
            </tr>
            <tr></tr>
            <tr>
                <td><em>{{ sheet.C6.v }}</em></td>
                <td><em>{{ d[0] }}</em></td>
            </tr>
            <tr>
                <td>{{ sheet.C7.v }}</td>
                <td><input type="text" v-model.trim.number="d[1]"></td>
            </tr>
            <tr>
                <td>{{ sheet.C8.v }}</td>
                <td><input type="text" v-model.trim.number="d[2]"></td>
            </tr>
            <tr>
                <td>{{ sheet.C9.v }}</td>
                <td><input type="text" v-model.trim.number="d[3]"></td>
            </tr>
            <tr>
                <td style="font-weight:700">{{ sheet.C11.v }}</td>
                <td></td>
            </tr>
            <tr>
                <td><em>{{ sheet.C12.v }}</em></td>
                <td><em>{{ d[4] }}</em></td>
            </tr>
            <tr>
                <td><em>{{ sheet.C13.v }}</em></td>
                <td><em>{{ d[5] }}</em></td>
            </tr>
            <tr>
                <td>{{ sheet.C14.v }}</td>
                <td></td>
            </tr>
            <tr>
                <td>{{ sheet.C15.v }}</td>
                <td><input type="text" v-model.trim.number="d[6]"></td>
            </tr>
            <tr>
                <td>{{ sheet.C16.v }}</td>
                <td><input type="text" v-model.trim.number="d[7]"></td>
            </tr>
            <tr>
                <td>{{ sheet.C17.v }}</td>
                <td><input type="text" v-model.trim.number="d[8]"></td>
            </tr>
            <tr>
                <td>{{ sheet.C18.v }}</td>
                <td><input type="text" v-model.trim.number="d[9]"></td>
            </tr>
            <tr>
                <td>{{ sheet.C19.v }}</td>
                <td><em>{{ d[10] }}</em></td>
            </tr>
            <tr>
                <td>{{ sheet.C20.v }}</td>
                <td></td>
            </tr>
            <tr>
                <td>{{ sheet.C21.v }}</td>
                <td><input type="text" v-model.trim.number="d[11]"></td>
            </tr>
            <tr>
                <td>{{ sheet.C22.v }}</td>
                <td><input type="text" v-model.trim.number="d[12]"></td>
            </tr>
            <tr>
                <td>{{ sheet.C23.v }}</td>
                <td><input type="text" v-model.trim.number="d[13]"></td>
            </tr>
            </tbody>
        </table>
        <button type="button" @click="saveTemplate" :value="'Окна ' + sheet.D3.v">Сохранить</button>
        </div>
    </div>
</template>

<script>
    module.exports = {
        data: function() {
            return {
                path: null,
                workbook: null,
                sheet: null,
                d: [],
                cortegeList: null,
            };
        },
        props: {
            file: Object,
        },
        methods: {
            handleFileChange: function(e) {
                this.path = e.target.files[0].name;
                var mod = this;
                var reader = new FileReader();
                reader.onload = function(e) {
                    var data = e.target.result;
                    data = new Uint8Array(data);
                    mod.readFromUint8(data);
                };
                reader.readAsArrayBuffer(e.target.files[0]);
            },
            readFromUint8: function (uint8) {
                this.workbook = XLSX.read(uint8, {type: 'array'});
                this.sheet = this.workbook.Sheets[this.workbook.SheetNames[0]];
                var sheet = this.sheet;
                var d = this.d;
                d[1] = sheet.D7?sheet.D7.v:0;
                d[2] = sheet.D7?sheet.D8.v:0;
                d[3] = sheet.D7?sheet.D9.v:0;
                d[0] = d[1] + d[2] + d[3];

                d[6] = sheet.D15?sheet.D15.v:0;
                d[7] = sheet.D16?sheet.D16.v:0;
                d[8] = sheet.D17?sheet.D17.v:0;
                d[9] = sheet.D18?sheet.D18.v:0;
                d[5] = d[6] + d[7] + d[8];

                d[11] = sheet.D21?sheet.D21.v:0;
                d[12] = sheet.D22?sheet.D22.v:0;
                d[13] = sheet.D23?sheet.D23.v:0;
                d[10] = d[11] + d[12];

                d[4] = d[5] + d[10] + d[9] + d[13];

                app.$data.windowsTemplateFillingmodalTitle = sheet.C2.v + " | " + sheet.D3.v;
            },
            saveTemplate: function (e) {
                var filename = this.path;
                console.log(this.sheet);
                this.sheet.D7 = this.cellValue(this.sheet.D7, this.d[1]);
                this.sheet.D8 = this.cellValue(this.sheet.D8, this.d[2]);
                this.sheet.D9 = this.cellValue(this.sheet.D9, this.d[3]);

                this.sheet.D15 = this.cellValue(this.sheet.D15, this.d[6]);
                this.sheet.D16 = this.cellValue(this.sheet.D16, this.d[7]);
                this.sheet.D17 = this.cellValue(this.sheet.D17, this.d[8]);
                this.sheet.D18 = this.cellValue(this.sheet.D18, this.d[9]);

                this.sheet.D21 = this.cellValue(this.sheet.D21, this.d[11]);
                this.sheet.D22 = this.cellValue(this.sheet.D22, this.d[12]);
                this.sheet.D23 = this.cellValue(this.sheet.D23, this.d[13]);

                this.sheet.D6 = this.cellValue(this.sheet.D6, this.d[0]);
                this.sheet.D12 = this.cellValue(this.sheet.D12, this.d[4]);
                this.sheet.D13 = this.cellValue(this.sheet.D13, this.d[5]);
                this.sheet.D19 = this.cellValue(this.sheet.D19, this.d[10]);
                var wbout = XLSX.write(this.workbook, {type:'buffer', bookType:"xlsx"});
                fs.writeFile(filename, wbout, function(err) {});
            },
            cellValue: function(cell, value) {
                cell = cell || {};
                cell.v = value;
                cell.w = value;
                cell.t = 'n';
                return(cell);
            },
            recalculate: function (id) {
                switch (id) {
                    case 0:  return(this.d[1] + this.d[2] + this.d[3]);
                    case 4:  return(this.d[5] + this.d[9] + this.d[10] + this.d[13]);
                    case 5:  return(this.d[6] + this.d[7] + this.d[8]);
                    case 10: return(this.d[11] + this.d[12]);
                    default: console.warn("Unexpected id in recalculate");
                    return(0);
                }
            },
            loadCortegeList: function () {
                if (localStorage.cortegeList) {
                    this.cortegeList = JSON.parse(localStorage.cortegeList);
                }
            },
            saveCortegeList: function () {
                localStorage.cortegeList = JSON.stringify(this.cortegeList);
            },
            fill: function (e) {
                var cortegeValue = JSON.parse(e.currentTarget.value);
                
                Vue.set(this.d, 1, cortegeValue[0]);
                Vue.set(this.d, 2, cortegeValue[1]);
                Vue.set(this.d, 3, cortegeValue[2]);

                Vue.set(this.d, 6, cortegeValue[3]);
                Vue.set(this.d, 7, cortegeValue[4]);
                Vue.set(this.d, 8, cortegeValue[5]);
                Vue.set(this.d, 9, cortegeValue[6]);

                Vue.set(this.d, 11, cortegeValue[7]);
                Vue.set(this.d, 12, cortegeValue[8]);
                Vue.set(this.d, 13, cortegeValue[9]);
            },
            remove: function (e) {
                var index = e.currentTarget.value;
                Vue.delete(this.cortegeList, index);
                this.saveCortegeList();
            },
        },
        watch: {
            'd.1': function (){
                this.d[0] = this.recalculate(0);
            },
            'd.2': function (){
                this.d[0] = this.recalculate(0);
            },
            'd.3': function (){
                this.d[0] = this.recalculate(0);
            },

            'd.6': function (){
                this.d[5] = this.recalculate(5);
            },
            'd.7': function (){
                this.d[5] = this.recalculate(5);
            },
            'd.8': function (){
                this.d[5] = this.recalculate(5);
            },

            'd.11': function (){
                this.d[10] = this.recalculate(10);
            },
            'd.12': function (){
                this.d[10] = this.recalculate(10);
            },

            'd.5': function (){
                this.d[4] = this.recalculate(4);
            },
            'd.9': function (){
                this.d[4] = this.recalculate(4);
            },
            'd.10': function (){
                this.d[4] = this.recalculate(4);
            },
            'd.13': function (){
                this.d[4] = this.recalculate(4);
            },
        },
        mounted: function () {
            this.loadCortegeList();
            /*if (file) {

            }*/
        },
    }
</script>

<style>
</style>
