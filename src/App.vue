<template>
   <div id="app">
   <v-app>
   
   <!-- <header> -->
      <!--menu-->
      <v-app-bar-nav-icon @click=" drawer= true">
          <v-toolbar-title></v-toolbar-title>

        <v-navigation-drawer v-model="drawer" fixed temorary>
          <v-list nav dense>
            <v-list-item-group>

              <v-list-item
                v-for = "(menuItem,index) in menuItems" :key ="index" >

                <v-list-item-title>{{menuItem.name}}</v-list-item-title>


              </v-list-item>

            </v-list-item-group>
          </v-list>

        </v-navigation-drawer>
        
      </v-app-bar-nav-icon>
      
    <!-- </header>   -->
      
      <!---->
      <v-main>
        <v-content>
          <v-container>
            <v-card  class="mx-auto" max-width="500" >
             
               
              <v-img
              src="./assets/main.png"
              >
              
              <v-app-bar id="logo" flat class="transparent">
                
                <!-- class="font-weight-bold text-center white--text pl-0 -->
                
                <v-toolbar-title class="logo-title">
                  Dr. Asset Planner
                </v-toolbar-title>

              </v-app-bar>
              
              </v-img>

             


              <v-card   flat width="200" class="ma-2" >
              
              <v-card-title></v-card-title>
 
              </v-card>


              
              <!-- ナビゲーション-->
              <v-card-subtitle dense>
              <!-- ↓ナビゲーション↓ -->


                <!--パンくず-->
               <div>
                  <v-breadcrumbs :items="items">
                    <template v-slot:divider>
                      <v-icon>mdi-forward</v-icon>
                    </template>
                  </v-breadcrumbs>
              
                  <!-- <v-breadcrumbs :items="items">
                    <template v-slot:divider>
                      <v-icon>mdi-chevron-right</v-icon>
                    </template>
                  </v-breadcrumbs> -->
                </div>
            
               </v-card-subtitle>

              <v-card-title id="title">
              <p><span style=font-size:15px;>AI<span data-icon="mdi-head-sync-outline"></span>が調べるので個人情報一切不要!!</span>
               <br>今と同じ家賃で買える家がわかる!?</p>
                
              </v-card-title>

            
              <v-card-text >

        <!--修正  -->
        <v-form ref="test_form">    
             
                  
                   
                    <!--マンション名-->
                    <v-col cols="12" >
                      <v-autocomplete  prepend-icon="mdi-home-assistant" outlined hide-details  label="物件名" placeholder="例：エステートマンション" v-model="pname"
                        :error-messages="pnameErrors" required @input="$v.pname.$touch()" @blur="$v.pname.$touch()">
                        ここに物件名のリスト？
                      </v-autocomplete>
                    </v-col>

              <v-row>

                    <!--家賃-->
                    <v-col cols="6">
                     <!-- prepend-icon="mdi-cash-usd" -->
                      <v-text-field prepend-icon="mdi-bitcoin" outlined hide-details color="orange" 
                        label="家賃" placeholder="例：90,000" suffix="円" 

                        input :value="inputRent"  @input="inputRent=$event"
                        :rules="[required, limit_length]"
                        

                        >
                        <!--↑:error-messages="rentErrors" required required @input="$v.rent.$touch()" @blur="$v.rent.$touch()" -->
                       </v-text-field>
                    </v-col>

                    <v-col cols="6" id="rent">  
                      <div class="progress-bar" :style="{ background: barColorRent, width: barWidthRent + 'px'}">入力してね!!{{messageRent}}</div>
                    </v-col>  

            </v-row>

            <v-row>
                   <!--広さ placeholder="例：30"-->
                    <v-col cols="6"> 
                      <v-text-field prepend-icon="mdi-arrow-expand"  outlined hide-details 
                      label="例：30"  suffix="平方" 
                      
                       input :value="input" @input="input=$event"
                       :rules="[required]" :style="{color:black}"
                       
                       id="check" maxlength="4" pattern="[0-9]{1,4}"
                       @onchange="inputCheck()"
                       
                      >
                     
                       
                        <!--  :error-messages="sizeErrors" required @input="$input_dd.$touch()" @blur="$v.size.$touch()" -->
                      
                      </v-text-field> 
                    </v-col>   

                    <v-col cols="6" id="size">  
                      <div class="progress-bar" :style="{ background: barColor, width: barWidth + 'px'}">入力してね!!{{message}}</div>
                    </v-col>   
             </v-row> 

              <v-col cols="6">
                <div class="frame" :style="{color:black}">入力内容：
                  <span id="output" :style="{color:black}"></span>
                </div>
               
                
              </v-col>  
                      
        
                    <!-- 利用規約 -->
                    <v-col cols="12">
                      <v-checkbox hide-details 
                       v-model="checkbox" 
                    

                      label="利用規約に同意"
                      :error-messages="checkboxErrors" 
                      required @change="$v.checkbox.$touch()" @blur="$v.checkbox.$touch()" > 

                     
                      </v-checkbox>
                    </v-col>
      
              </v-form>

            

                  <!-- 調べるボタン -->
                    <v-col cols="12" id="container">
                      <v-btn block class="btn-open"
                     
                       @click="submit" >
                       
                        <span class="mgr-10">今すぐ調べる 
                         <v-icon dark right>mdi-checkbox-marked-circle</v-icon>
                        </span>  
                        </v-btn>
                    </v-col> 
           
                    
                 
               
              </v-card-text>
              
            </v-card>


          </v-container><!-- main end -->
          <!---->

          <!-- -->
        </v-content>

        <!-- ふっだーリスト名: links -->
        <v-card height="150" color="grey lighten-4">
          <v-footer padless absolute>
            <v-row justify="center" no-gutters>
              <v-btn v-for="link in links" :key="link" text rounded class="my-2">
                {{ link }}
              </v-btn>
              <v-col class="grey lighten-4 py-4 text-center font-weight-light" cols="12">
                &copy; {{ new Date().getFullYear() }} — Estate Technologies Inc.
              </v-col>
            </v-row>
          </v-footer>
        </v-card>

      </v-main>
    </v-app>
  </div>
</template>

<script>


export default {
  name: 'App',
 
data : function(){
  return{

     // menu
    drawer:false,

   // 家賃
        inputRent :"", 
    // watch :{ inputRent→同じ名前

        newinputRent : '',
    
        messageRent : '弱',
       barWidthRent : '60',
        barColorRent: '#ff1919', 
        

  //広さ
        input: "",
        // watch :{ input→同じ名前

        newinput:'',

        message: '弱',
        barWidth: '60',
        barColor: '#ff1919',


         // 入力規則
        required : value => !!value || "必ず入力してください", // 入力必須の制約
        limit_length : value => value.length <= 10 || "10文字以内で入力してください" ,// 文字数の制約
        
        
        checkbox : false,
       
   



    menuItem : [
      {
      name : 'Dr.asset',
      url : ''
      },
      {
      name : 'Dr.asset',
       url : ''
      },
      {
      name : 'Dr.asset',
       url : ''
      },
      {
      name : 'Dr.asset',
       url : ''
      },
    ],

// ぱんくず
    items: [
        {
          text: '検索(TOP)',
          disabled: true,
          href: '',
        },
        {
          text: '結果',
          disabled: false,
          href: '',
        },
      ],


    links: [
      
          "LOGO",
          "当サイトについて",
          "お問い合わせ",
          "プライバシーポリシー",
          "クッキーポリシー",
        
    ],

  }
},

 methods: {

inputCheck () {
	var form = document.getElementById("check");
	var form_value = form.value;
  
  if (form_value.match(/[0-9]{1,4}/g) != form_value ) {
    form.value = '';
    document.getElementById('output').innerHTML = '4桁以内の数値を入力してください。';
  } else {
	document.getElementById('output').innerHTML = 'OKです。';  
  }
  document.getElementById('output').innerHTML = form_value;
},


    // 送信を試みるメソッド（「送信する」がクリックされたときに呼ばれる）
      submit() {
       // ↓***** 新規追加 & 修正

          if (this.$refs.test_form.validate() && (this.checkbox === true) ){
           // すべてのバリデーションが通過したときのみ
          // if文の中に入る
        
            this.success = true;
            alert('送信成功！！');

          }else if(this.checkbox === false) {
            alert('利用規約に同意してください');
          }else{
          this.success = false;
          }
      }
    },



watch:{

// 家賃 

       inputRent:{    
           handler: function (newinputRent) {

            this.newinputRent = newinputRent

         
              // lengthじゃないと動かない
        
        var vm = this;
        var length;
        length = vm.inputRent.length;
        this.barWidthRent = 60 + length * 15
        
          if (length <= 3) {
            vm.messageRent = 'あと少し';
            vm.barColorRent = '#ff1919';
          }else if (length <= 6 ) {
            vm.messageRent = 'OK';
            vm.barColorRent = '#ffee33';
          }else if (length <= 8 ){
            vm.messageRent =  'それ以上は入力できない';
            vm.barColorRent = '#40ffd9';
          }
        },
      deep: true,
},

 
// 広さ
        input: function (newinput) {

        this.newinput = newinput            
        
        var vm = this;

        var length;

        
        length = vm.input.length;
        this.barWidth = 60 + length * 15
        
          if (length < 1) {
            vm.message = 'もうすぐ';
            vm.barColor = '#ff1919';
          }else if (length <= 3) {
            vm.message = '完了';
            vm.barColor = '#ffee33';
          }else{
            vm.message =  'それ以上は入力できない';
            vm.barColor = '#40ffd9';
          }
          
        }
          
        
      },
 }


  



    
  

 
    
  </script>

  <style>
  @import url("https://fonts.googleapis.com/css?family=Questrial");


@font-face {
	font-family: 'MyFont' !important;
	/* src: url(./assets/myfont.ttf) !important; */
  src: url(http://dekasu.net/fonts/?p=1,3,9)
}


#logo .logo-title {
  font-size: 20px ;
  font-family: "MyFont" !important; 
  font-family: "UI Gothic" !important;
  color:white;
  /* font-family: "UI Gothic" !important;
  font-family: "Questrial" !important;
  font-family:  "sans-serif" !important; */

}
  

  .title span {
    /* font-size: 10px ; */
    display: block !important;
  }



.theme--light.v-text-field>.v-input__control>.v-input__slot:before {
    border-color: #ff9800;
}

.theme--light.v-label {
    color: #ff9800;
}

.theme--light.v-input:not(.v-input--is-disabled) input, .theme--light.v-input:not(.v-input--is-disabled) textarea {
    color: #ff9800;
}

/* */
#rent .progress-bar {
  transition : width 0.4s linear;
  background-color : 0.4s linear;

  height:50px;
  border-radius: 3px;
}

/*  */
#size .progress-bar {
  transition : width 0.4s linear;
  background-color : 0.4s linear;

  height:50px;
  border-radius: 3px;
}




 

 /* 調べるボタン */
 
#container {
  font-family: "Yu Gothic", YuGothic, Verdana, 'Hiragino Kaku Gothic ProN','Hiragino Kaku Gothic Pro', 'ヒラギノ角ゴ Pro W3', 'メイリオ', Meiryo, sans-serif;
  
  text-align: center;
  padding-top: 40px;
}
#container .btn-open {
  display: inline-block;
  width: 180px;
  height:50px;
  text-align: center;
  background-color: #48c74f;
  font-size: 25px;
  line-height: 52px;
  color: #FFF;
  text-decoration: none;
  font-weight: bold;
  border: 0.6px solid #3f3001;
  position: relative;
  overflow: hidden;
  z-index: 1;
}
#container .btn-open:after{
  width: 100%;
  height: 0;
  content:"";
  position: absolute;
  top: 50%;
  left: 50%;
  background : #FFF;
  opacity: 0;
  transform: translateX(-50%) translateY(-50%) rotate(45deg);
  transition: .2s;
  z-index: -1;
}
#container .btn-open:hover{
  color: #48c74f;
}
#container .btn-open:hover:after{
  height: 240%;
  opacity: 1;
}
#container .btn-open:active:after{
  height: 340%;
  opacity: 1;
}

#container .btn-open .mgr-10 {
    margin-right : 220px;
    margin-left : 220px;
}

  .f1 {
  font-family: "Questrial";
  font-weight:bold;
  }
  


  


  </style>
  
       
      

