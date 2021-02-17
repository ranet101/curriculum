<template>
    <div id="app">
        <div class="container">
            <div class="te_porter">
                <span class="typed-text" v-html="typeValue"></span>
            </div>
            <!-- span class="cursor" :class="{'typing': typeStatus}">&nbsp;</span -->
            <input id="prompt" v-model="promptStr" type="text" v-on:keyup.enter="tryc()" value="">
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
                                "Raul Garcia Ameyugo",
                                "",
                                "Programador Web fullstack",
                                "",
                                "Para ver los detalles del curriculum escriba comandos tipo VER DATOS PERSONALES o VER EXPERIENCIA LABORAL.",
                                "",
                                "Escriba AYUDA para ver las diferentes opciones."
                                ],
                "ayuda":[
                        "limpiar -> Limpia la pantalla",
                        "ver ->  Para ver al detalle una parte o todo el curriculum. "
                        ]
            }],
            typingSpeed: 45,
            erasingSpeed: 100,
            charIndex: 0,
            promptStr:"",
            allowedComands:["limpiar"]
        }
    },
    methods: {
        typeText() {
            if(this.charIndex < this.typeStr.length) {
                this.typeValue += this.typeStr.charAt(this.charIndex);
                this.charIndex += 1;
                this.typeStatus = true;
                setTimeout(this.typeText, this.typingSpeed);
            }else{
                this.typeValue += "<br />";
                this.charIndex = 0;
                this.typeStatus = false;
            }
        },
        eraseText() {
            if(this.charIndex > 0) {
                if(!this.typeStatus)
                    this.typeStatus = true;
                this.typeValue = this.typeArray[this.typeArrayIndex].substring(0, this.charIndex - 1);
                this.charIndex -= 1;
                setTimeout(this.eraseText, this.erasingSpeed);
            } else {
                this.typeStatus = false;
                this.typeArrayIndex += 1;
                if(this.typeArrayIndex >= this.typeArray.length)
                this.typeArrayIndex = 0;
                setTimeout(this.typeText, this.typingSpeed + 1000);
            }
        },
        tryc(){
            console.log(this.promptStr);
        }
    },
    watch:{
      typeValue: function () {
          if(!this.typeStatus){
              document.getElementById("prompt").focus();
          }
      }
    },
    created() {
        let presentacion = this.arrayTextos[0]["presentacion"];
        let that = this;
        presentacion.forEach(function (d, i) {
            var myVar = setInterval(function () {
                if(!that.typeStatus){
                    that.typeStr = d;
                    that.typeText();
                    clearInterval(myVar)
                }
            }, 100);
        });
    }
}
</script>

<style lang="scss">//
    @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

    body {
        margin: 0;
        bottom: 0;
        padding: 0;
    }

    #app {
        font-family: 'Press Start 2P', cursive;
        font-size:10px;
        color: #fff;
        background-color: #241E17;
        width: 100%;
        min-height: 100vh;
    }

    .te_porter{
        width: 100%;
        position: fixed;
        bottom: 35px;
    }

    input#prompt{
        padding:0 0 10px 0;
        font-family: 'Press Start 2P', cursive;
        font-size:10px;
        background-color: #241E17;
        border: none;
        color: #fff;
        position: fixed;
        bottom: 0;
        width: 100%;
        height: 15px;
        &:focus{
            outline:none;
        }
    }


    .container {
        position: absolute;
        bottom: 0;
        padding: 10px
    }

    span{
        line-height: 150%;
        &.cursor {
            display: inline-block;
            width: 4px;
            background-color: #fff;
            animation: cursorBlink 1s infinite;
        }
        &.cursor.typing {
            animation: none;
        }
    }

    @keyframes cursorBlink {
        49% { background-color: #fff; }
        50% { background-color: transparent; }
        99% { background-color: transparent; }
    }
</style>
