<style lang="less" scoped>
    #change_contab{
        .el-tabs{
            box-shadow: none;
        }
        .tabBody{
            .el-row{
                margin: 10px 0;
                .long{
                    .el-select{
                        width:350px;
                    }
                }
                .el-input-number{
                    width: 110px;
                }
            }
        }
        .bottom{
            width: 100%;
            text-align: center;
            margin-top: 5px;
            position: relative;
            .value{
                font-size: 18px;
                vertical-align: middle;
            }
        }
    }
</style>
<template>
    <div id="change_contab">
        <el-tabs type="border-card">
            <el-tab-pane>
                <span slot="label"><i class="el-icon-date"></i> {{text.Minutes.name}}</span>
                <div class="tabBody">
                    <el-row>
                        <el-radio v-model="minute.cronEvery" label="2">{{text.Minutes.interval[0]}}
                            <el-input-number size="small" v-model="minute.incrementIncrement" :min="5" :max="59"></el-input-number>
                            {{text.Minutes.interval[1]}}
                            <el-input-number size="small" v-model="minute.incrementStart" :min="0" :max="59"></el-input-number>
                            {{text.Minutes.interval[2]||''}}
                        </el-radio>
                    </el-row>
                    <el-row>
                        <el-radio class="long" v-model="minute.cronEvery" label="3">{{text.Minutes.specific}}
                            <el-select size="small" multiple v-model="minute.specificSpecific">
                                <el-option v-for="val in 60" :key="$index" :value="val-1">{{val-1}}</el-option>
                            </el-select>
                        </el-radio>
                    </el-row>
                </div>
            </el-tab-pane>
            <el-tab-pane>
                <span slot="label"><i class="el-icon-date"></i> {{text.Hours.name}}</span>
                <div class="tabBody">
                    <el-row>
                        <el-radio v-model="hour.cronEvery" label="1">{{text.Hours.every}}</el-radio>
                    </el-row>
                    <el-row>
                        <el-radio v-model="hour.cronEvery" label="2">{{text.Hours.interval[0]}}
                            <el-input-number size="small" v-model="hour.incrementIncrement" :min="0" :max="23"></el-input-number>
                            {{text.Hours.interval[1]}}
                            <el-input-number size="small" v-model="hour.incrementStart" :min="0" :max="23"></el-input-number>
                            {{text.Hours.interval[2]}}
                        </el-radio>
                    </el-row>
                    <el-row>
                        <el-radio class="long" v-model="hour.cronEvery" label="3">{{text.Hours.specific}}
                            <el-select size="small" multiple v-model="hour.specificSpecific">
                                <el-option v-for="val in 24" :key="$index" :value="val-1">{{val-1}}</el-option>
                            </el-select>
                        </el-radio>
                    </el-row>
                    <el-row>
                        <el-radio v-model="hour.cronEvery" label="4">{{text.Hours.cycle[0]}}
                            <el-input-number size="small" v-model="hour.rangeStart" :min="0" :max="23"></el-input-number>
                            {{text.Hours.cycle[1]}}
                            <el-input-number size="small" v-model="hour.rangeEnd" :min="parseInt(hour.rangeStart) || 0" :max="23"></el-input-number>
                            {{text.Hours.cycle[2]}}
                        </el-radio>
                    </el-row>
                </div>
            </el-tab-pane>
            <el-tab-pane>
                <span slot="label"><i class="el-icon-date"></i> {{text.Day.name}}</span>
                <div class="tabBody">
                    <el-row>
                        <el-radio v-model="day.cronEvery" label="1">{{text.Day.every}}</el-radio>
                    </el-row>
                    <el-row>
                        <el-radio v-model="day.cronEvery" label="2">{{text.Day.intervalWeek[0]}}
                            <el-input-number size="small" v-model="week.incrementIncrement" :min="1" :max="7"></el-input-number>
                            {{text.Day.intervalWeek[1]}}
                            <el-select size="small" v-model="week.incrementStart">
                                <el-option v-for="val in 7" :key="$index" :label="text.Week[val-1]" :value="'' + (val-1)"></el-option>
                            </el-select>
                            {{text.Day.intervalWeek[2]}}
                        </el-radio>
                    </el-row>
                    <el-row>
                        <el-radio v-model="day.cronEvery" label="3">{{text.Day.intervalDay[0]}}
                            <el-input-number size="small" v-model="day.incrementIncrement" :min="1" :max="31"></el-input-number>
                            {{text.Day.intervalDay[1]}}
                            <el-input-number size="small" v-model="day.incrementStart" :min="1" :max="31"></el-input-number>
                            {{text.Day.intervalDay[2]}}
                        </el-radio>
                    </el-row>
                    <el-row>
                        <el-radio class="long" v-model="day.cronEvery" label="4">{{text.Day.specificWeek}}
                            <el-select size="small" multiple v-model="week.specificSpecific">
                                <el-option v-for="val in 7"
                                           :key="$index"
                                           :label="text.Week[val-1]"
                                           :value="val-1"
                                ></el-option>
                            </el-select>
                        </el-radio>
                    </el-row>
                    <el-row>
                        <el-radio class="long" v-model="day.cronEvery" label="5">{{text.Day.specificDay}}
                            <el-select size="small" multiple v-model="day.specificSpecific">
                                <el-option v-for="val in 31" :key="$index" :value="val">{{val}}</el-option>
                            </el-select>
                        </el-radio>
                    </el-row>
                    <template v-if="false">
                        <el-row>
                            <el-radio v-model="day.cronEvery" label="6">{{text.Day.lastDay}}</el-radio>
                        </el-row>
                        <el-row>
                            <el-radio v-model="day.cronEvery" label="7">{{text.Day.lastWeekday}}</el-radio>
                        </el-row>
                        <el-row>
                            <el-radio v-model="day.cronEvery" label="8">{{text.Day.lastWeek[0]}}
                                <el-select size="small" v-model="day.cronLastSpecificDomDay">
                                    <el-option v-for="val in 7" :key="$index" :label="text.Week[val-1]" :value="val"></el-option>
                                </el-select>
                                {{text.Day.lastWeek[1]||''}}
                            </el-radio>
                        </el-row>
                        <el-row>
                            <el-radio v-model="day.cronEvery" label="9">
                                <el-input-number size="small" v-model="day.cronDaysBeforeEomMinus" :min="1" :max="31"></el-input-number>
                                {{text.Day.beforeEndMonth[0]}}
                            </el-radio>
                        </el-row>
                        <el-row>
                            <el-radio v-model="day.cronEvery" label="10">{{text.Day.nearestWeekday[0]}}
                                <el-input-number size="small" v-model="day.cronDaysNearestWeekday" :min="1" :max="31"></el-input-number>
                                {{text.Day.nearestWeekday[1]}}
                            </el-radio>
                        </el-row>
                        <el-row>
                            <el-radio v-model="day.cronEvery" label="11">{{text.Day.someWeekday[0]}}
                                <el-input-number size="small" v-model="week.cronNthDayNth" :min="1" :max="5"></el-input-number>
                                <el-select size="small" v-model="week.cronNthDayDay">
                                    <el-option v-for="val in 7" :key="$index" :label="text.Week[val-1]" :value="val"></el-option>
                                </el-select>
                                {{text.Day.someWeekday[1]}}
                            </el-radio>
                        </el-row>
                    </template>
                </div>
            </el-tab-pane>
            <el-tab-pane>
                <span slot="label"><i class="el-icon-date"></i> {{text.Month.name}}</span>
                <div class="tabBody">
                    <el-row>
                        <el-radio v-model="month.cronEvery" label="1">{{text.Month.every}}</el-radio>
                    </el-row>
                    <el-row>
                        <el-radio v-model="month.cronEvery" label="2">{{text.Month.interval[0]}}
                            <el-input-number size="small" v-model="month.incrementIncrement" :min="0" :max="12"></el-input-number>
                            {{text.Month.interval[1]}}
                            <el-input-number size="small" v-model="month.incrementStart" :min="0" :max="12"></el-input-number>
                        </el-radio>
                    </el-row>
                    <el-row>
                        <el-radio class="long" v-model="month.cronEvery" label="3">{{text.Month.specific}}
                            <el-select size="small" multiple v-model="month.specificSpecific">
                                <el-option v-for="val in 12" :key="$index" :label="val" :value="val"></el-option>
                            </el-select>
                        </el-radio>
                    </el-row>
                    <el-row>
                        <el-radio v-model="month.cronEvery" label="4">{{text.Month.cycle[0]}}
                            <el-input-number size="small" v-model="month.rangeStart" :min="1" :max="12"></el-input-number>
                            {{text.Month.cycle[1]}}
                            <el-input-number size="small" v-model="month.rangeEnd" :min="1" :max="12"></el-input-number>
                        </el-radio>
                    </el-row>
                </div>
            </el-tab-pane>
        </el-tabs>
        <div class="bottom">
            <span class="value">{{this.cron}}</span>
            <el-button type="primary" @click="change">{{text.Save}}</el-button>
            <el-button type="primary" @click="close">{{text.Close}}</el-button>
        </div>
    </div>
</template>
<script>
    import Language from '../language/index'
    export default {
    name:'vueCron',
        props:['data', 'expression', 'i18n'],
    data(){

        return {
            minute:{
                cronEvery:'3',
                incrementStart:'0',
                incrementIncrement:'5',
                rangeStart:'',
                rangeEnd:'',
                specificSpecific:[0],
            },
            hour:{
                cronEvery:'3',
                incrementStart:'0',
                incrementIncrement:'5',
                rangeStart:'',
                rangeEnd:'',
                specificSpecific:[9],
            },
            day:{
                cronEvery:'1',
                incrementStart:'1',
                incrementIncrement:'1',
                rangeStart:'',
                rangeEnd:'',
                specificSpecific:[],
                cronLastSpecificDomDay:1,
                cronDaysBeforeEomMinus:'',
                cronDaysNearestWeekday:'',
            },
            week:{
                cronEvery:'1',
                incrementStart:'0',
                incrementIncrement:'1',
                specificSpecific:[],
                cronNthDayDay:1,
                cronNthDayNth:'1',
            },
            month:{
                cronEvery:'1',
                    incrementStart:'1',
                    incrementIncrement:'2',
                rangeStart:'',
                rangeEnd:'',
                specificSpecific:[],
            },
            output:{
                minute:'',
                hour:'',
                day:'',
                month:'',
                Week:'',
                },
                defaultSet: false
        }
    },
    watch:{
        data(){
            this.rest(this.$data);
        }
    },
    computed: {
        text(){
            return Language[this.i18n||'en']
        },
        minutesText() {

            let minutes = '';
            let cronEvery=this.minute.cronEvery;
            switch (cronEvery.toString()){
                case '2':
                    const increment = this.minute.incrementStart+'-59';
                    minutes = (increment === '0-59' ? '*' : increment) + '/' + this.minute.incrementIncrement;
                    break;
                case '3':
                    this.minute.specificSpecific.map(val=> {
                        minutes += val+','
                    });
                    minutes = minutes.slice(0, -1);
                    break;
            }
            return minutes;
        },
        hoursText() {
            let hours = '';
            let cronEvery=this.hour.cronEvery;
            switch (cronEvery.toString()){
                case '1':
                    hours = '*';
                    break;
                case '2':
                    const increment = this.hour.incrementStart+'-23';
                    hours = (increment === '0-23' ? '*' : increment) + '/' + this.hour.incrementIncrement;
                    break;
                case '3':
                    this.hour.specificSpecific.map(val=> {
                        hours += val+','
                    });
                    hours = hours.slice(0, -1);
                    break;
                case '4':
                    hours = this.hour.rangeStart+'-'+this.hour.rangeEnd;
                    break;
            }
            return hours;
        },
        daysText() {
            let days='';
            let cronEvery=this.day.cronEvery;
            switch (cronEvery.toString()){
                case '1':
                    break;
                case '2':
                case '4':
                case '11':
                    days = '*';
                    break;
                case '3':
                    const increment = this.day.incrementStart + '-31';
                    days = (increment === '1-31' ? '*' : increment) + '/' + this.day.incrementIncrement;
                    break;
                case '5':
                    this.day.specificSpecific.map(val=> {
                        days += val+','
                    });
                    days = days.slice(0, -1);
                    break;
                case '6':
                    days = "L";
                    break;
                case '7':
                    days = "LW";
                    break;
                case '8':
                    days = this.day.cronLastSpecificDomDay + 'L';
                    break;
                case '9':
                    days = 'L-' + this.day.cronDaysBeforeEomMinus;
                    break;
                case '10':
                    days = this.day.cronDaysNearestWeekday+"W";
                    break
            }
            return days;
        },
        weeksText() {
            let weeks = '';
            let cronEvery=this.day.cronEvery;
            switch (cronEvery.toString()){
                case '1':
                case '3':
                case '5':
                    weeks = '*';
                    break;
                case '2':
                    const increment = this.week.incrementStart + '-6';
                    weeks = (increment === '0-6' ? '*' : increment) + '/' + this.week.incrementIncrement;
                    break;
                case '4':
                    this.week.specificSpecific.map(val=> {
                        weeks += val+','
                    });
                    weeks = weeks.slice(0, -1);
                    break;
                case '6':
                case '7':
                case '8':
                case '9':
                case '10':
                    weeks = "*";
                    break;
                case '11':
                    weeks = this.week.cronNthDayDay+"#"+this.week.cronNthDayNth;
                    break;
            }
            return weeks;
        },
        monthsText() {
            let months = '';
            let cronEvery=this.month.cronEvery;
            switch (cronEvery.toString()){
                case '1':
                    months = '*';
                    break;
                case '2':
                    const increment = this.month.incrementStart + '-12';
                    months = (increment === '1-12' ? '*' : increment) + '/' + this.month.incrementIncrement;
                    break;
                case '3':
                    this.month.specificSpecific.map(val=> {
                        months += val+','
                    });
                    months = months.slice(0, -1);
                    break;
                case '4':
                    months = this.month.rangeStart+'-'+this.month.rangeEnd;
                    break;
            }
            return months;
        },
        cron(){
            let cronText = '';
            if (this.defaultCronExpression) {
                cronText = `${this.minutesText||'*'} ${this.hoursText||'*'} ${this.daysText||'*'} ${this.monthsText||'*'} ${this.weeksText||'*'}`;

                this.$emit('cron-updated', cronText);
            }
            return cronText;
        },
        defaultCronExpression() {
            if (!this.defaultSet && this.expression) {
                this.parseExpression(this.expression);
                this.defaultSet = !this.defaultSet;
            }
            return this.defaultSet;
        }
    },
    methods: {
            parseMinute(minute) {
                if (minute.indexOf('/') > 0) {
                    this.minute.cronEvery = '2';

                    const parts = minute.split('/');

                    if (parts[0] === '*') {
                        this.minute.incrementStart = '0';
                    } else if (parts[0].match(/\d+-\d+/)) {
                        const match = parts[0].match(/(\d+)-\d+/);
                        this.minute.incrementStart = match[1] || '0';
                    }

                    if (parseInt(parts[1]) > 0) {
                        this.minute.incrementIncrement = parts[1];
                    }
                } else if (minute.indexOf(',') > 0 || (minute.indexOf(',') === -1 && parseInt(minute) >= 0)) {
                    this.minute.cronEvery = '3';
                    this.minute.specificSpecific = minute.split(',').map(m => parseInt(m.trim())).filter(h => !isNaN(h));
                }
            },
            parseHour(hour) {
                if (hour === '*') {
                    this.hour.cronEvery = '1';
                } else if (hour.indexOf('/') > 0) {
                    this.hour.cronEvery = '2';

                    const parts = hour.split('/');

                    if (parts[0] === '*') {
                        this.hour.incrementStart = '0';
                    } else if (parts[0].match(/\d+-\d+/)) {
                        const match = parts[0].match(/(\d+)-\d+/);
                        this.hour.incrementStart = match[1] || '0';
                    }

                    if (parseInt(parts[1]) > 0) {
                        this.minute.incrementIncrement = parts[1];
                    }
                } else if (hour.match(/\d+-\d+/)) {
                    const parts = hour.split('-');
                    this.hour.cronEvery = '4';
                    this.hour.rangeStart = parts[0] || '0';
                    this.hour.rangeEnd   = parts[1] || '23';
                } else if (hour.indexOf(',') > 0 || (hour.indexOf(',') === -1 && parseInt(hour) >= 0)) {
                    this.hour.cronEvery = '3';
                    this.hour.specificSpecific = hour.split(',').map(h => parseInt(h.trim())).filter(h => !isNaN(h));
                }

            },
            parseDay(day) {
                if (day === '*') {
                    this.day.cronEvery = '1';
                } else if (day.indexOf('/') > 0) {
                    this.day.cronEvery = '3';

                    const parts = day.split('/');

                    if (parts[0] === '*') {
                        this.day.incrementStart = '1';
                    } else if (parts[0].match(/\d+-\d+/)) {
                        const match = parts[0].match(/(\d+)-\d+/);
                        this.day.incrementStart = match[1] || '1';
                    }

                    if (parseInt(parts[1]) > 0) {
                        this.day.incrementIncrement = parts[1];
                    }
                } else if (day.indexOf(',') > 0 || (day.indexOf(',') === -1 && parseInt(day) >= 1)) {
                    this.day.cronEvery = '5';
                    this.day.specificSpecific = day.split(',').map(h => parseInt(h.trim())).filter(h => !isNaN(h));
                }
            },
            parseMonth(month) {
                if (month === '*') {
                    this.month.cronEvery = '1';
                } else if (month.indexOf('/') > 0) {
                    this.hour.cronEvery = '1';

                    const parts = month.split('/');

                    if (parts[0] === '*') {
                        this.month.incrementStart = '1';
                    } else if (parts[0].match(/\d+-\d+/)) {
                        const match = parts[0].match(/(\d+)-\d+/);
                        this.month.incrementStart = match[1] || '1';
                    }

                    if (parseInt(parts[1]) > 0) {
                        this.month.incrementIncrement = parts[1];
                    }
                } else if (month.match(/\d+-\d+/)) {
                    const parts = month.split('-');
                    this.month.cronEvery = '4';
                    this.month.rangeStart = parts[0] || '1';
                    this.month.rangeEnd   = parts[1] || '12';
                } else if (month.indexOf(',') > 0 || (month.indexOf(',') === -1 && parseInt(month) >= 1)) {
                    this.month.cronEvery = '3';
                    this.month.specificSpecific = month.split(',').map(h => parseInt(h.trim())).filter(h => !isNaN(h));
                }
            },
            parseWeek(week) {
                if (week === '*') {
                    // do nothing
                } else if (week.indexOf('/') > 0) {
                    this.day.cronEvery = '2';

                    const parts = week.split('/');

                    if (parts[0] === '*') {
                        this.week.incrementStart = '1';
                    } else if (parts[0].match(/\d+-\d+/)) {
                        const match = parts[0].match(/(\d+)-\d+/);
                        this.week.incrementStart = match[1] || '1';
                    }

                    if (parseInt(parts[1]) > 0) {
                        this.week.incrementIncrement = parts[1];
                    }
                } else if (week.indexOf(',') > 0 || (week.indexOf(',') === -1 && parseInt(week) >= 1)) {
                    this.day.cronEvery = '4';
                    this.week.specificSpecific = week.split(',').map(h => parseInt(h.trim())).filter(h => !isNaN(h));
                }
            },
            parseExpression(expression) {
                const elements = expression.split(' ');
                console.log('elements', elements);
                if (elements.some(e => e === '') === true) {
                    return;
                }
                if (elements.length !== 5) {
                    return;
                }
                console.log('parsing input');

                try {
                    this.parseMinute(elements[0]);
                    this.parseHour(elements[1]);
                    this.parseDay(elements[2]);
                    this.parseMonth(elements[3]);
                    this.parseWeek(elements[4]);
                } catch (e) {
                    // silent fail
                }
            },
        getValue(){
            return this.cron;
        },
        change(){
            this.$emit('change',this.cron);
            this.close();
        },
        close(){
            this.$emit('close');
        },
        rest(data){
            for(let i in data){
                if(data[i] instanceof Object){
                    this.rest(data[i])
                }else{
                    switch(typeof data[i]){
                        case 'object':data[i]=[];break;
                        case 'string':data[i]='';break;
                    }
                }
            }
        }
    }
}</script>
