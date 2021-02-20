<template>
    <div id="app" @click="promptFocus()" class="default">
        <div class="container">
            <div class="te_porter">
                <span class="typed-text" v-html="typeValue"></span>
            </div>
            <!-- span class="cursor" :class="{'typing': typeStatus}">&nbsp;</span -->
            <input id="prompt" v-model="promptStr" type="text" @keyup.enter="launchCommand()" value="" autocomplete="off" autofocus>
        </div>
    </div>
</template>

<script>

export default {
    name: 'app',
    data: () => {
        return {
            typeValue: '',
            typeStatus: false,
            typeStr:"",
            arrayTextos:[{
                "presentacion":[
                    "<h1>Raul Garcia Ameyugo</h1>",
                    "<h2>Programador Web / Fullstack</h2>",
                    "Para ver los detalles del curriculum escriba comandos tipo VER DATOS PERSONALES o VER EXPERIENCIA.",
                    "Escriba AYUDA para ver las diferentes opciones.",
                    "Escriba IMPRIMIR para ver el curriculum en formato pdf.",
                    ""
                ],
                "ayuda":[
                    "VER: Para ver al detalle una parte o todo el curriculum.",
                    "IMPRIMIR: Imprime/Descarga version en pdf.",
                    "LIMPIAR: Limpia la pantalla.",
                    "TRANSFORMA: Cambia el estilo del terminal",
                    "-/+: Sube baja la velocidad de escritura.",
                    ""
                ],
                "ver":[{
                    "experiencia":[
                        "SDP Ibérica, Barcelona/Sint-Niklaas (BE) — Técnico de sistemas informáticos<br />SEPTIEMBRE 2004 - ABRIL 2007<br />- Instalación y mantenimiento de sistemas informáticos para la gestión de diferentes software desarrollados por la empresa. Periodo trabajado en el extranjero, entre Bélgica y Francia.",
                        "",
                        "Compuspar, Barcelona — Administrador de sistemas informáticos<br />MAYO 2007 - ENERO 2008<br />- Gestión de los sistemas informáticos y redes telemáticas. Mantenimiento de servidores basados en Microsoft y Linux y configuración de redes y accesos remotos.",
                        "",
                        "Freelance/autonomo, Barcelona — Desarrollador web<br />ENERO 2008 - ENERO 2013<br />- Etapa rica en experiencia y aprendizaje. Formé parte de todo tipo de proyectos y desarrollé desde landing pages hasta gestores propios de contenido. Me gusta hacer hincaié en las colaboraciones con empresas dedicadas al social media. Normalmente desarrollo de herramientas para seguimiento de redes sociales.",
                        "",
                        "Educacionline, Barcelona — Desarrollador web<br />ENERO 2013 - AGOSTO 2013<br />- Colaboración con la empresa para la creación de un sistema de multi blogging enfocado a promocionar sus diferentes cursos. Wordpress, fullstack.",
                        "",
                        "Rosa del Vents, Barcelona — Desarrollador web<br />AGOSTO 2013 - JULIO 2015<br />- Mantenimiento del ecosistema web de los diferentes departamentos del grupo RV. Mantenimiento completo y creación de utilidades según las necesidades. Full-stack y gestores de contenidos.",
                        "",
                        "NeXTreT, Barcelona — Desarrollador front-end<br />JULIO 2015 - SEPTIEMBRE 2018<br />- CCMA/TV3, mantenimiento y desarrollo en el ecosistema ccma.cat. Fullstack con tendencia a front-end<br />- Ticketmaster, mantenimiento de sistema de compra de entradas y abonos realizada con tecnología front-end.",
                        "",
                        "DuplexMarketing, Barcelona — Desarrollador full-stack<br />SEPTIEMBRE 2018 - FEBRERO 2019<br />- Desarrollo full-stack en una empresa de marketing. Mayoritariamente promociones basadas en un sistema web con codeigniter.",
                        "",
                        "Gemweb, Barcelona — Desarrollador full-stack/backend<br />FEBRERO 2019 - ACTUALIDAD<br />- Desarrollo full-stack, pero sobre todo backend sobre una aplicacion de gestion energetica. Mantenimiento y nuevos desarrollos.",
                        ""
                    ],
                    "idiomas":[
                        "Francés. Competencia bilingüe o nativa.",
                        "Inglés. Competencia profesional completa.",
                        "Catalán. Competencia profesional completa.",
                        ""
                    ],
                    "tecnologias":[
                        "Javascript, jQuery, Backbone.js, Vue.js.",
                        "PHP, MySQL/SQL, XML, JSON",
                        "Codeigniter, Laravel",
                        "Wordpress, Joomla",
                        "Responsive Web Design",
                        "Automatización de tareas.",
                        "Versionado con repositorios SVN y Git",
                        "Desarrollo de tests (unitarios y funcionales)",
                        ""
                    ],
                    "datos":[{
                        "personales":[
                            "Raul Garcia Ameyugo",
                            "08026 Barcelona (BARCELONA)",
                            "(34) 653 73 76 78","ranet101@gmail.com"],
                        "academicos":[
                            "Estudios cursados en Francia<br />Equivalente BUP y COU",
                            "1992, HENDAYA (FR)",
                            "",
                            "Centro de formación profesional Bidasoa, Irún",
                            "Formación profesional de grado superior - Electricidad y Electrónica",
                            "1992 - 1997, IRÚN",
                            "",
                            "Cebanc CDEA, Donosti, San Sebastián",
                            "Ciclo formativo de grado superior, Informática, programación y administración de sistemas informáticos.",
                            "1998 - 2000, DONOSTI, SAN SEBASTIÁN",
                            "",
                            "Centro BIT, Barcelona",
                            "Master - Webmaster",
                            "2001 - 2002, BARCELONA",
                            "",
                        ],
                        "interes":[
                            "Carnet de conducir B",
                            "Idiomas:",
                            "Francés. Competencia bilingüe o nativa.",
                            "Inglés. Competencia profesional completa.",
                            "Catalán. Competencia profesional completa.",
                            ""
                        ]
                    }]
                }]
            }],
            commands:[{
                "limpiar":{},
                "ayuda":{},
                "imprimir":{},
                "transforma":{},
                "ver":[{
                    "datos":[{
                        "personales":{},
                        "academicos":{},
                        "interes":{}
                    }],
                    "experiencia":[],
                    "idiomas":[],
                    "tecnologias":[]
                }],
                "+":{},
                "-":{},
            }],
            typingSpeed: 5,
            erasingSpeed: 100,
            charIndex: 0,
            promptStr:"",
            command:"",
            param1:"",
            param2:"",
            level:""
        }
    },
    methods: {
        waitForPrint(str){
            var that = this;
            var intervalo = setInterval(function () {
                if(!that.typeStatus){
                    that.typeStr = str;
                    that.printFromString();
                    clearInterval(intervalo);
                }
            }, 100);
        },
        printFromString() {
            if(this.charIndex < this.typeStr.length) {
                this.typeValue += this.typeStr.charAt(this.charIndex);
                this.charIndex += 1;
                this.typeStatus = true;
                setTimeout(this.printFromString, this.typingSpeed);
            }else{
                this.typeValue += "<br />";
                this.typeValue += " ";
                this.charIndex = 0;
                this.typeStatus = false;
            }
        },
        printFromArray(array){
            let that = this;
            array.forEach(function (d, i) {
                that.waitForPrint(d);
            });
        },
        launchCommand(){
            let splited = this.promptStr.split(" ");
            this.command = splited[0].toLowerCase();
            this.level = 1;
            if(typeof splited[1]!="undefined"){
                this.param1 = splited[1].toLowerCase();
                this.level = 2;
            }
            if(typeof splited[2]!="undefined"){
                this.param2 = splited[2].toLowerCase();
                this.level = 3;
            }
            this.typeValue+="<br />";
            for(var i=1;i<=this.level;i++){
                if(!this.checkCommand(i)){
                    this.printErrors(i);
                    this.typeValue+="Use uno de los siguientes comandos:";
                    this.printOptions(i);
                    this.resetPrompt();
                    return;
                }
            }
            this.printCommand();
            this.launchAction();
            this.resetPrompt();
        },
        checkCommand(level){
            let element;
            switch(level){
                case 1:
                    element = this.commands[0][this.command];
                    break;
                case 2:
                    element = this.commands[0][this.command][0][this.param1];
                    break;
                case 3:
                    element = this.commands[0][this.command][0][this.param1][0][this.param2];
                    break;
            }
            if(typeof element==="undefined"){
                return false;
            }else{
                return true;
            }
        },
        printCommand(){
            let str;
            switch(this.level){
                case 1:
                    str="Comando: <i>"+this.command.toUpperCase()+"</i>";
                    break;
                case 2:
                    str="Comando <i>"+this.command.toUpperCase()+" "+this.param1.toUpperCase()+"</i>.";
                    break;
                case 3:
                    str="Comando <i>"+this.command.toUpperCase()+"  "+this.param1.toUpperCase()+" "+this.param2.toUpperCase()+"</i>.";
                    break;
            }
            this.typeValue+=str+"<br/>";
        },
        printErrors(errorOn){
            let str;
            switch(errorOn){
                case 1:
                    str="<span style='color:red'>Error:</span> comando <span style='color:red'><i>"+this.command.toUpperCase()+"</i></span> no encontrado.";
                    break;
                case 2:
                    str="<span style='color:red'>Error:</span> comando "+this.command.toUpperCase()+" <span style='color:red'><i>"+this.param1.toUpperCase()+"</i></span> no encontrado.";
                    break;
                case 3:
                    str="<span style='color:red'>Error:</span> comando "+this.command.toUpperCase()+"  "+this.param1.toUpperCase()+" <span style='color:red'><i>"+this.param2.toUpperCase()+"</i></span> no encontrado.";
                    break;
            }
            this.typeValue+=str+"<br />";
        },
        printOptions(level){
            let options;
            switch(level){
                case 1:
                    options= this.commands[0];
                    break;
                case 2:
                    options= this.commands[0][this.command][0];
                    break;
                case 3:
                    options= this.commands[0][this.command][0][this.param1][0];
                    break;
            }
            for (var option in options){
                this.waitForPrint(option.toUpperCase());
            }
        },
        promptFocus(){
            document.getElementById("prompt").focus();
        },
        resetPrompt(){
            this.typeValue+="<br />";
            this.promptFocus();
            this.promptStr = "";
        },
        launchAction(){
            switch(this.command){
                case "limpiar":
                    this.limpiar();
                    break;
                case "ayuda":
                    this.ayuda();
                    break;
                case "imprimir":
                    this.imprimir();
                    break;
                case "ver":
                    this.ver();
                    break;
                case "+":
                    this.subirVelocidad();
                    break;
                case "-":
                    this.bajaVelocidad();
                    break;
                case "transforma":
                    this.transforma();
                    break;
            }
        },
        limpiar(){
            this.typeValue="";
        },
        ayuda(){
            this.printFromArray(this.arrayTextos[0]["ayuda"]);
        },
        imprimir(){
            window.open('Curriculum_vitae_Raul_Garcia.pdf','_target');
        },
        ver(){
            if(this.level < 2){
                this.waitForPrint("El comando VER no puede usarse solo. Pruebe VER con:");
                this.printOptions(2);
                return;
            }else if(this.level==2 && this.param1=="datos"){
                this.waitForPrint("El comando VER DATOS no puede usarse solo. Pruebe VER DATOS con:");
                this.printOptions(3);
                return;
            }
            if(this.level==2){
                this.printFromArray(this.arrayTextos[0][this.command][0][this.param1]);
            }else if(this.level==3){
                this.printFromArray(this.arrayTextos[0][this.command][0][this.param1][0][this.param2]);
            }
        },
        subirVelocidad(){
            this.typingSpeed+=5;
            this.waitForPrint("Velocidad de printado actual: "+this.typingSpeed+" ms");
        },
        bajaVelocidad(){
            if(this.typingSpeed>=10){
                this.typingSpeed-=5;
                this.waitForPrint("Velocidad de printado actual: "+this.typingSpeed+" ms");
            }else{
                this.typingSpeed=5;
                this.waitForPrint("Velocidad de printado minima alcanzada: "+this.typingSpeed+" ms");
            }
        },
        transforma(){
            let old_class = document.getElementById('app').className, new_class;
            console.log(old_class);
            switch (old_class) {
                case "default":
                    new_class = "style2";
                    break;
                case "style2":
                    new_class = "style3";
                    break;
                default:
                    new_class = "default";
                    break;
            }
            document.getElementById('app').className = '';
            document.getElementById("app").classList.add(new_class);
            this.waitForPrint("Estilo aplicado: "+new_class);
        }
    },
    watch:{
        typeValue: function () {
            if(!this.typeStatus){
                this.promptFocus();
            }
        }
    },
    created() {
        this.printFromArray(this.arrayTextos[0]["presentacion"]);
    }
}
</script>

<style lang="scss">//
    @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Inconsolata:wght@300&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=VT323&display=swap');

    body {
        margin: 0;
        bottom: 0;
        padding: 0;
    }

    #app {
        font-size:10px;
        width: 100%;
        min-height: 100vh;
        &.default{
            font-family: 'Press Start 2P', cursive;
            color: #fff;
            font-size:10px;
            background-color: #241E17;
            input#prompt{
                font-family: 'Press Start 2P', cursive;
                color: #fff;
                font-size:10px;
                background-color: #241E17;
            }
            span{
                line-height: 175%;
            }
        }
        &.style2{
            font-family: 'VT323', monospace;
            color: #ffB000;
            font-size:18px;
            background-color: #000;
            input#prompt{
                font-family: 'VT323', monospace;
                color: #ffB000;
                font-size:18px;
                background-color: #000;
            }
        }
        &.style3{
            font-family: 'Inconsolata', monospace;
            color: #00FF66;
            background-color: #000;
            font-size:14px;
            input#prompt{
                font-family: 'Inconsolata', monospace;
                color: #00FF66;
                background-color: #000;
                font-size:14px;
            }
        }

        .container {
            position: absolute;
            bottom: 0;
            padding: 10px;
            .te_porter{
                width: 100%;
                position: fixed;
                bottom: 35px;
            }
        }

        input#prompt{
            padding:0 0 10px 0;
            border: none;
            position: fixed;
            bottom: 0;
            width: 100%;
            height: 15px;
            &:focus{
                outline:none;
            }
        }
    }

    @keyframes cursorBlink {
        49% { background-color: #fff; }
        50% { background-color: transparent; }
        99% { background-color: transparent; }
    }
</style>
