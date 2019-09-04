<template>

    <div>

    <b-container class="bv-example-row">
        <b-row>
            <b-col>
                <b-alert show v-if="!end_">
                    <h2>Простой и быстрый опрос</h2>
                    <div>Пройдите его и мы сможем детально рассчитать цену Вашего заказа и сообщить её Вам.</div>
                    <div><strong>Вопрос</strong>: <strong>{{index}}</strong> из <strong>{{totalLength}}</strong></div>
                </b-alert>
                <b-alert show v-else>
                    <h2>Как с Вами связаться?</h2>
                    <div>Заполните данные и мы пришлем приблизительную стоимость наших услуг за выбранные Вами позиции.</div>
                </b-alert>
            </b-col>
        </b-row>
    </b-container>

    <div class="question-box-container">
        <b-jumbotron v-if="showForm===0">
            <template slot="lead">
                {{currentQuestion.question}}
            </template>
            <hr class="my-4">
            <div v-if="currentQuestion.type=='single'">
                <b-list-group
                    v-for="(answer, i) in answers"
                    :key="i"
                    @click="selectAnswer(i)"
                    class="cp"
            >
                <b-form-radio v-model="selected" name="some-radios" :value="answer">{{answer}}</b-form-radio>
            </b-list-group>
            <div :class="setClass()" class="mt-3 selected">

                <!--end if/else

            если ifYes, то должно быть только ДВА ответа!
            Если отвечаем на первый, то срабатывает условие (показываем input, textarea ... )

            Если есть ifYes и текстовое поле нужно вводить

            -->
                <div v-if="currentQuestion.ifYes===true
                &&currentQuestion.ifYesDataType=='textarea'
                &&this.selectedIndex!==null
                &&this.selectedIndex==0"
                >
                    <hr>
                    <div class="tl sml">{{currentQuestion.ifYesDataText}}:</div>
                    <b-form-textarea
                            v-model="text"
                            rows="3"
                            max-rows="6"
                            @input="inputUpdate()"
                    ></b-form-textarea>
                </div>

                <!--Если есть ifYes и input поле нужно вводить-->
                <div
                        v-if="currentQuestion.ifYes===true
                &&currentQuestion.ifYesDataType=='input'
                &&this.selectedIndex!==null
                &&this.selectedIndex==0"
                >
                    <hr>
                    <div class="tl sml">{{currentQuestion.ifYesDataText}}:</div>

                    <b-form-input v-model="text" @input="inputUpdate()" :type="currentQuestion.ifYesDataTypeInput"></b-form-input>

                </div>

                Выбранный Вами вариант: <strong>{{ selected }}</strong></div>
            </div>

            <div v-else>
                <b-list-group
                    v-for="(answer, i) in answers"
                    :key="i"
                    @click="selectAnswer(i)"
                    class="cp"
                >
                    <b-form-checkbox-group id="checkbox-group-2" v-model="selected" name="flavour-2">
                        <b-form-checkbox :value="answer" @click="variantFunc()">{{answer}}</b-form-checkbox>
                    </b-form-checkbox-group>
                </b-list-group>

                <div :class="setClass()" class="mt-3 selected"><span v-html="variantFunc()"></span></div>
            </div>

            <b-button
                    @click="next(),clearClass()"
                    variant="success"
                    :disabled="isDisabled()"
            >Следующий</b-button>

        </b-jumbotron>

        <b-container v-else>
            <b-form @submit.stop.prevent="onSubmit">
                <div class="sml tl">Заполните все поля:</div>

    <div class="fc">
                <b-form-group>
                    <div class="tl"><svg viewBox="0 0 428 428" width="18"><use xlink:href="ic.svg#user"></use></svg> Ваше имя</div>
                    <b-form-input
                            id="input-1"
                            name="name"
                            v-model="$v.form.name.$model"
                            :state="$v.form.name.$dirty ? !$v.form.name.$error : null"
                            aria-describedby="input-1-live-feedback"
                            placeholder="Ваше имя"
                    ></b-form-input>

                    <b-form-invalid-feedback id="input-1-live-feedback">
                        Введите имя от двух символов. Например: Ян или Ирина
                    </b-form-invalid-feedback>
                </b-form-group>


                <b-form-group>
                    <div class="tl"><svg viewBox="0 0 208 208" width="18"><use xlink:href="ic.svg#phone"></use></svg> Ваш телефон</div>
                    <b-form-input
                            id="input-2"
                            name="phone"
                            v-model="$v.form.phone.$model"
                            :state="$v.form.phone.$dirty ? !$v.form.phone.$error : null"
                            aria-describedby="input-2-live"
                            type="tel"
                            @keyup="validPhone"
                            @keydown="validPhone"
                            @paste="validPhone"
                            @click="validPhone"
                            placeholder="Ваш номер телефона"
                    ></b-form-input>

                    <b-form-invalid-feedback id="input-2-live">
                        Введите Ваш контактный номер телефона. Например: +38 066 553 00 00
                    </b-form-invalid-feedback>
                </b-form-group>
    </div>

                <b-form-group>
                    <div class="tl"><svg viewBox="0 0 485 485" width="18"><use xlink:href="ic.svg#email"></use></svg> Ваша почта</div>
                    <b-form-input
                            id="input-3"
                            name="email"
                            v-model="$v.form.email.$model"
                            :state="$v.form.email.$dirty ? !$v.form.email.$error : null"
                            aria-describedby="input-3-live"
                            type="email"
                            placeholder="Ваш email"
                    ></b-form-input>

                    <b-form-invalid-feedback id="input-3-live">
                        Введите Вашу электронную почту. Например: irina@gmail.com
                    </b-form-invalid-feedback>
                </b-form-group>


                <b-form-group>
                    <div class="tl"><svg viewBox="0 0 60 60" width="18"><use xlink:href="ic.svg#text"></use></svg> Ваше обращение</div>
                    <b-form-textarea
                            rows="7"
                            id="input-4"
                            v-model="$v.form.userText.$model"
                            :state="$v.form.userText.$dirty ? !$v.form.userText.$error : null"
                            aria-describedby="input-4-live"
                            @input="symbols()"
                            placeholder="Текст обращения"
                    ></b-form-textarea>

                    <div class="sml tl" v-if="$v.form.userText.$model!=null">{{symbols()}}</div>

                    <b-form-invalid-feedback id="input-4-live">
                        Введите Ваше обращение. Минимальная длина: 7 символов.
                    </b-form-invalid-feedback>

                    <div class="error" v-if="error!='false'">{{error}}</div>
                    <div class="ok" v-if="ok!='false'">{{ok}}</div>
                    <div class="pr dn loadingQ">

                        <b-button
                                type="submit"
                                variant="primary"
                                @click.prevent="onSubmit()"

                        >Отправить</b-button>

                    </div>

                    <div class="tl sml">Если возникли трудности, напишите нам на <a href="mailto:go@inverser.pro">go@inverser.pro</a></div>

                </b-form-group>
            </b-form>
        </b-container>

    </div>

    </div>

</template>

<script>

    import { validationMixin } from 'vuelidate'
    import { required, email, minLength, maxLength } from 'vuelidate/lib/validators'

    export default {
        mixins: [validationMixin],
        props:{
            currentQuestion: Object,
            index: Number,
            totalLength: Number,
            next: Function
        },
        data(){
            return{
                selectedIndex: null,
                selected:'',
                class_:'show',
                disabledBtn:'',
                disabledBtnSend:true,
                variant:'Выбранный Вами вариант',
                userAnsvers:{},
                text:'',
                showForm:0,
                form: {
                    name: null,
                    phone: null,
                    email: null,
                    userText: null
                },
                end_:false,
                error:'',
                ok:''
            }
        },
        validations: {
            form: {
                name: {required,minLength: minLength(2),maxLength: maxLength(30)},
                phone: {required,minLength: minLength(7),maxLength: maxLength(21)},
                email: {required,minLength: minLength(7),maxLength: maxLength(33),email},
                userText: {minLength: minLength(5),maxLength: maxLength(2350)}
            }
        },
        methods:{
            selectAnswer(index){
                this.selectedIndex = index
            },
            setClass(){
                if(this.selected!=''){
                    return this.class_
                }
            },
            inputUpdate(){
                this.userAnsvers[`${this.index}-${this.currentQuestion.ifYes}`]=this.text
            },
            clearClass(){
                /*Add new value to our userAnswers*/
                if(this.index==this.totalLength){
                    this.end_=this.showForm=true
                }
                this.userAnsvers[`${this.index}. ${this.currentQuestion.question}`]=`${this.selected}`
                this.selectedIndex=null
                return  this.selected = this.text=''
            },
            isDisabled(){
                if(this.selected==''){
                    return this.disabledBtn='disabled'
                }
            },
            variantFunc(){
                if(Array.isArray(this.selected)){
                    if(this.selected.length>1){
                        let str=''
                        this.selected.forEach(function(el) {
                            str+=`<li>${el}</li>`;
                        });
                        return this.variant = `Выбранные Вами варианты: <ul style="text-align:left">${str}</ul>`
                    }else{
                        return this.variant = `Выбранный Вами вариант: ${this.selected}`
                    }
                }
            },
            symbols(){
                if(this.form.userText!=null) {
                    if(this.form.userText.length>4)return 'Осталось символов: ' + (2350 - parseInt(this.form.userText.length))
                }
            },
            validPhone(){
                if(this.form.phone!=null){
                    if(/[A-Za-zА-Яа-яЁё]/.test(this.form.phone)){
                        this.form.phone=this.form.phone.replace(/[A-Za-zА-Яа-яЁё]/,"")
                        this.form.phone=this.form.phone.replace(/ {1,}/g," ")
                        return false
                    }else{
                        return true
                    }
                }
            },
            onSubmit() {
                this.$v.form.$touch()
                if (this.$v.form.$anyError) {/*если есть какие-л ошибки в заполненных данных формы (имя, email, тел...)*/
                    console.log('has-errors')
                    return false
                }
                console.log('Ok');

                function formEncode(obj) {/*эта функция позволит преобразовать JS Object в валидные данные для POST-запроса*/
                    var str = [];
                    for(let p in obj)
                        str.push(encodeURIComponent(p) + "=" + encodeURIComponent(obj[p]));
                    return str.join("&");
                }

                let sendQuiz=false

                if(sendQuiz){
                    this.ok='Вы уже отправили данные.'
                    return false
                }

                fetch("response.json")
                .then(r =>  r.json().then(data => ({status: r.status, body: data})))
                .then(
                    obj => {if(obj.status==200){
                        console.log(obj.body)
                        console.log('response is ok')
                        this.ok=obj.body.ok
                        this.error=obj.body.error
                        sendQuiz=true

                        const element = document.querySelector(".container form");
                        element.classList.add("pr");
                    }}
                );
            }
        },
        computed:{
            answers(){
                let answers=[...this.currentQuestion.answers]
                return answers
            }
        }/*,
        watch:{

        },
        mounted(){
            ifError()
        }*/
    }
</script>

<style>
    .cp,.custom-control-label{cursor:pointer}
    .tr,.list-group,.show{transition:all .5s linear}
    .b3,.list-group{border-radius:3px}
    .tl,.d-block{text-align:left}
    .sml{font-size:.8rem}

    .list-group{margin-bottom:15px}
    .btn{margin:10px}
    .selected{opacity:0}
    .show{opacity:1}
    .custom-control,.custom-control-label{display:block;text-align:left;padding:4px}
    .list-group{line-height:1;margin-bottom:0;padding:0 0 0 4px;box-shadow: inset 0 0 0 1px #dedede}
    .list-group:hover{background:#fff}
    .custom-control-label::before,.custom-control-label::after{left:0}
    .custom-control-label{padding-left:2rem}
    button[type="submit"]{display: flex;
        justify-content: center;
        width: 100%;
        margin: 10px 0;
        text-transform: uppercase;}

    .loadingQ::after,.loadingQ::before{content:"";position:absolute;top:0;right:0;bottom:0;left:0;width:50%;height:1px;background:linear-gradient(90deg,transparent 0,#fff 50%,transparent 100%);margin:1rem 0 0 0;animation:aqz 1s infinite}
    @keyframes aqz{50%{transform:translateX(100%)}}
    .loadingQ::after{background:rgba(255,255,255,.2);width:auto;height:auto;animation:none;margin:0}
</style>