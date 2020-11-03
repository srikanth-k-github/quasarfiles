<template>
<q-page>

  <!-- row 1 -->
    <div class="  q-col-gutter-sm  q-py-sm"  >
   <!-- Quest -->
    <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
    <q-card class=" ">
    <q-banner inline-actions class="text-info bg-primary">
      <span class="text-h6 q-mr-sm">Add </span>
      <q-badge color="" style="background-color:#13cfb4" class="text-blue-10 text-caption q-pa-sm">{{ crqtype }}</q-badge> 
      <span class="text-h6 q-ml-sm">questions </span>
      <template v-slot:action>
            <q-btn dense round glossy icon="help" color="info" text-color="amber-3">
      <q-menu>
        <div class="row no-wrap q-pa-none  round"  >
          <div class="column q-pb-sm" style="width:300px">
            <div class="text-caption q-ml-sm q-mt-sm">  
            <div class="text-weight-bold text-accent text-center">Pick a Question type</div>
            <q-separator/> 
            <q-list >
              <q-item class="q-pa-none" clickable v-close-popup @click.stop="checkQClick('Multiple Choice')">
                <q-btn flat color="grey-7" icon="list"  />
                <q-item-section>
                  <q-item-label>MCQ</q-item-label>
                  <q-item-label caption>One or More Anwers to Question</q-item-label>
                </q-item-section>
              </q-item>
              <q-item class="q-pa-none" clickable v-close-popup @click.stop="checkQClick('Match The Following')">
                <q-btn flat color="grey-7"  icon="view_list" />
                <q-item-section>
                  <q-item-label>Match</q-item-label>
                  <q-item-label caption>Match given block items</q-item-label>
                </q-item-section>                
              </q-item>              
              <q-item class="q-pa-none" clickable v-close-popup @click.stop="checkQClick('Fill In the Blank')">
                <q-btn flat color="grey-7"  icon="subject" />
                <q-item-section>
                  <q-item-label>FIB</q-item-label>
                  <q-item-label caption>Fill In the Blank(s)</q-item-label>
                </q-item-section>                
              </q-item> 
              <q-item class="q-pa-none" clickable v-close-popup @click.stop="checkQClick('Short Answer')">
                <q-btn flat color="grey-7"  icon="description"/>
                <q-item-section>
                  <q-item-label>OEQ</q-item-label>
                  <q-item-label caption>Open End or Short Answer </q-item-label>
                </q-item-section>                
              </q-item>
              <q-item class="q-pa-none" clickable v-close-popup @click.stop="checkQClick('Long Answer')">
                <q-btn flat color="grey-7"  icon="menu_book"/>
                <q-item-section>
                  <q-item-label>STA</q-item-label>
                  <q-item-label caption>Story or Essay or Long Answer </q-item-label>
                </q-item-section>                
              </q-item>              
            </q-list>                            
           </div>
        </div>
        </div>
        </q-menu>
        </q-btn>
      </template>
    </q-banner>    
          <q-tabs
            v-model="tabqpaper"
            dense inline-label
            class="text-grey"
            active-color="primary"
            indicator-color="primary"
            align="justify"
          >
            <q-tab no-caps name="question" :class="tabqpaper=='question'?'text-primary':''" icon="help_outline"
                   :label="tablabel">
            </q-tab>
            <q-tab no-caps name="modeltestpaper" :class="tabqpaper=='modeltestpaper'?'text-primary':''" class="q-pa-xs" icon="content_paste" label="2.Test Paper">
            </q-tab>
          </q-tabs>
          <q-separator/>

          <q-tab-panels v-model="tabqpaper" animated>
            <q-tab-panel name="question" class="q-pa-sm">
                <div v-if="crqtype=='Multiple Choice'">
                <quest-mcq></quest-mcq>
                </div>
                <div v-if="crqtype=='Match The Following'"> 
                <quest-match></quest-match>  
                </div>                  
                <div v-if="crqtype=='Fill In the Blank'"> 
                <quest-fib-new :prvw.sync="prvwtext"></quest-fib-new>  
                </div>
                <div v-if="crqtype=='Short Answer'"> 
                <quest-oeq></quest-oeq>  
                </div>
                <div v-if="crqtype=='Long Answer'"> 
                <quest-sta></quest-sta>  
                </div>   
                               
            </q-tab-panel> 

            <q-tab-panel name="modeltestpaper" class="q-pa-sm">
                <quest-prepare></quest-prepare>
											 
            </q-tab-panel>   
                         
          </q-tab-panels>  
        
        </q-card>
        </div><!-- Quest end -->

 
    <!-- user table -->    
  </div>  <!-- row 1 end -->

    <div class="  q-col-gutter-sm  q-py-sm" >
   <!-- user table -->
   <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
      <q-card class=""> 
        <q-banner inline-actions class="text-info bg-primary">
          <span class="text-h6" v-if="mysettings.dashgroupname">{{mysettings.dashgroupname}} Users</span>        
          <span class="text-h6 " v-else>Add Users</span>        
        <template v-slot:action>  
          <div class="row ">
<!--             <single-file-upload 
            :propthisfile.sync="thisfile" 
            :propmfs="mfs"
            :propfiletypes="filetypes"
            :propprogress="thisfileprogress"
            :propcolor="info"
            title="Import .CSV disabled"
            disabled  size="sm" 
            class="q-mr-md">
            </single-file-upload> -->
            <q-btn color="info" text-color="secondary" dense glossy round  size="sm" icon="refresh" class="q-pa-xs q-mr-md" @click.stop="getGroupUsers()"/>
            <q-btn color="info" text-color="secondary" dense glossy round  size="sm" icon="person_add" class="q-pa-xs"/>
          
        <q-menu>
        <div class="row no-wrap q-pa-md"> 
          <div class="column items-center">
            <q-input v-model="adduser.name" placeholder="Full Name e.g. Guru Vidya"  dense type="text"                           maxlength="50"
                          :rules="[val => !!val || 'Input required']" />
            <q-input v-model="adduser.phone" placeholder="Phone e.g. 9840910400"  dense type="telephone"                           maxlength="50"
                          :rules="[val => !!val || 'Input required']"/>
            <q-input v-model="adduser.email" placeholder="guruvidya@guruttam.com"  dense type="email"                           maxlength="50"
                          :rules="[val => !!val || 'Input required']"/>
            <q-separator class="q-mb-xs"/>
            <q-btn
              color="primary"
              label="Save"
              push
              size="sm"
              v-close-popup
              @click.stop="saveOneUser()"
            />
          </div>
        </div>
      </q-menu>
        </q-btn>
        </div>    
      </template>          
        </q-banner>
          <q-card-section class="q-pa-none">
            <div class="text-subtitle q-pa-md" style="min-height:450px" v-if="mysettings.dashgroupname==''">
              <p class="text-caption" style="">
                 Users details will appear here after activiating or revisiting the group
              </p> 
 
            </div>
            <q-table class="no-shadow"
                     :data="resultarr"
                     title="Users" 
                     :hide-header="usermode === 'grid'"
                     :columns="usrcolumns"
                     row-key="email"
                     :filter="userfilter"
                     :pagination.sync="pagination"
                     v-if="mysettings.dashgroupname"
            >
              <template v-slot:top-right="props">
                <q-input outlined dense debounce="300" v-model="userfilter" placeholder="Search">
                  <template v-slot:append>
                    <q-icon name="search"/>
                  </template>
                </q-input>

                <q-btn
                  flat
                  round
                  dense
                  :icon="props.inFullscreen ? 'fullscreen_exit' : 'fullscreen'"
                  @click="props.toggleFullscreen"
                  v-if="usermode === 'list'" class="q-px-sm"
                >
                  <q-tooltip
                    :disable="$q.platform.is.mobile"
                    v-close-popup
                  >{{props.inFullscreen ? 'Exit Fullscreen' : 'Toggle Fullscreen'}}
                  </q-tooltip>
                </q-btn>
 
              </template>
            </q-table>
          </q-card-section>
        </q-card> 
        </div>
    </div> 

  <!-- row 2 --> <!--v-if="activegroup"-->
    <div class="  q-col-gutter-sm  q-py-sm" >
      <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
        <q-card> 
    <q-banner inline-actions class="text-info bg-primary">
      <span class="text-h6">Recent </span>
      <template v-slot:action>
       
        <!-- recent history btn -->
        <q-btn icon="school" rounded dense push class="info">
          <q-menu>
           <div class="column q-pa-md ">
            <div class="row  rounded-borders q-gutter-md">
              <q-input  v-model="sdate" size="sm" label="Test from"  
                class="q-ml-lg q-mt-sm" style="max-width:120px">
                <template v-slot:append>
                  <q-icon name="event" class="cursor-pointer" color="green">
                    <q-popup-proxy transition-show="scale" transition-hide="scale">
                      <q-date v-model="sdate" mask="DD-MM-YYYY" />
                    </q-popup-proxy>
                  </q-icon>
                </template> 
              </q-input>

              <q-input  v-model="edate"  size="sm" label="Test upto"   
                class="q-ml-lg q-mt-sm" style="max-width:120px">
                <template v-slot:append>
                  <q-icon name="event" class="cursor-pointer" color="red">
                    <q-popup-proxy transition-show="scale" transition-hide="scale">
                      <q-date v-model="edate" mask="DD-MM-YYYY" />
                    </q-popup-proxy>
                  </q-icon>
                </template> 
              </q-input>             
              </div>
              <div class="row q-pa-sm ">
              <q-input class="" style="max-width:18%" dense type="number" v-model="limit" label="Limit" placeholder="e.g.5" /> 
              <q-space/>
              <q-btn flat dense v-close-popup class="q-mt-sm bg-warning" icon="history" @click="clickHistory()" label-left >Recent</q-btn>
              </div>
              </div>
          </q-menu> 
        </q-btn> 
        <!-- recent history btn end -->
          <q-btn icon="mail" rounded dense push  class="" @click.stop="getGroupDoubts()"/>
         <q-btn icon="notifications" rounded dense push  @click.stop="getRecentMCQTakers()"/>
      </template>
    </q-banner>
 
          <q-tabs
            v-model="tabrecent"
            dense inline-label
            class="text-grey"
            active-color="primary"
            indicator-color="primary"
            align="justify"
          >
            <q-tab no-caps name="paper" :class="tabrecent=='paper'?'text-primary':''" icon="school" label="Test Papers">
             &nbsp; &nbsp; <q-badge color="red" floating >{{limit}}</q-badge>
            </q-tab>
            <q-tab no-caps name="message" :class="tabrecent=='message'?'text-primary':''" class="q-pa-xs" icon="mail" label="Message">
             &nbsp; &nbsp; <q-badge color="red" floating >{{rmcqdoubtsarr.length}}</q-badge>
            </q-tab>
            <q-tab no-caps name="notification" :class="tabrecent=='notification'?'text-primary':''" icon="notifications"
                   label="Notification">
             &nbsp; &nbsp; <q-badge color="red" floating>{{this.notifycount}}</q-badge>
            </q-tab>
          </q-tabs>

          <q-separator/>
          <q-tab-panels v-model="tabrecent" animated>
            <q-tab-panel name="paper" class="q-pa-sm">
              <q-list class="rounded-borders" separator>
                <q-item clickable @click="toggleRow(index)" 
                        :class="testpaper.checked?'bg-grey-8':''"
                        v-ripple v-for="(testpaper,index) in tpaper" :key="index">  
                  <q-item-section avatar>
<!--                     <q-avatar> 
                      <img :src="'statics/guruvidyaimages/' + testpaper.avatar" >
                    </q-avatar> -->
                    <q-btn size="sm" flat round glossy icon="school" class="bg-accent text-white" 
                        :title="testpaper.name">
                      <q-badge :color="testpaper.mock?'primary':'red-4'" floating>{{index+1}}</q-badge>
                    </q-btn>
                  </q-item-section> <!-- :src="'statics/guruvidyaimages/' + qimages"  --> 
                  <q-item-section>
                    <q-item-label lines="1">
                      <q-icon 
                      name = 'insert_drive_file'
                      :class="iconifyq(testpaper.status)" 
                      size="xs"
                      />{{testpaper.name}}
                    </q-item-label> 
                  </q-item-section>

                  <q-item-section side>
                   <!-- <div class="text-grey-8 q-gutter-xs"> 
                      <q-chip square><q-avatar color="primary" text-color="white">{{testpaper.totalquest}}</q-avatar></q-chip>   
                      <q-chip square><q-avatar color="positive" text-color="white">{{testpaper.totalmarks}}</q-avatar></q-chip>   
                    </div>   -->
                    <q-item-label caption lines="1">
                       <span class=" "> 
                         <q-separator vertical="" /> 
                         <q-badge outline class="q-mr-xs" align="middle" color="info">{{testpaper.totalquest}}q</q-badge>
                         <q-badge outline class="q-mr-xs" align="middle" color="teal">{{testpaper.totalmarks}}m</q-badge>
                          <!-- {{testpaper.sdate}}   -->
                        </span>
                    </q-item-label>
                    <q-item-label caption lines="2">
                       <span class=" "> 
                         <q-separator vertical="" /> 
                         <!-- <q-badge class="q-mr-xs" align="middle" color="negative">{{testpaper.tevent}}</q-badge> -->
                         <q-badge class="q-mr-xs" align="middle" color="grey-7" v-if="testpaper.tevent=='Elapsed'">{{testpaper.tevent}}</q-badge>
                         <q-badge class="q-mr-xs" align="middle" color="deep-orange" v-else-if="testpaper.tevent=='Shortly'">{{testpaper.tevent}}</q-badge>
                         <q-badge class="q-mr-xs" align="middle" color="warning" v-else-if="testpaper.tevent=='This Week'">{{testpaper.tevent}}</q-badge>
                         <q-badge class="q-mr-xs" align="middle" color="positive" v-else-if="testpaper.tevent=='Later'">{{testpaper.tevent}}</q-badge>
                          <!-- {{testpaper.sdate}}   -->
                        </span>
                    </q-item-label>
                    <q-item-label caption lines="3">
                       <span class=" ">
                         <q-btn @click.stop="alertTpDelete(testpaper.name,testpaper.id)" icon="delete"/> 
                        </span>
                    </q-item-label>                                             
<!--                     <q-item-label caption lines="2">
                      <span class="" >{{testpaper.duration}} Mi. {{testpaper.totquest}} Qs. {{testpaper.totmarks}} Mrk.</span>
                    </q-item-label>   -->                   
                  </q-item-section>
                </q-item>
              </q-list>

            </q-tab-panel>

            <q-tab-panel name="message" class="q-pa-sm">
              <q-item v-for="msg in rmcqdoubtsarr" :key="msg.id" clickable v-ripple>
                <q-item-section avatar>
                  <q-avatar>
                    <q-icon name="message" color="info"/>
                  </q-avatar>
                </q-item-section>

                <q-item-section>
                  <q-item-label>{{ msg.doubt }}</q-item-label>
                  <q-item-label caption lines="1">{{ msg.name }} </q-item-label> 
                </q-item-section>

                <q-item-section side>
                  <q-item-label caption lines="1">{{ msg.fname }} </q-item-label>               

                  <q-item-label caption lines="2">{{ msg.doubton }} 
                    <q-btn dense icon="reply" flat class="red" />
                  </q-item-label>
                    
                </q-item-section>
              </q-item>
            </q-tab-panel>

            <q-tab-panel name="notification" class="q-pa-sm">
              <q-list v-for="(rmcqtaker,idx) in rmcqtakeresarr" :key='idx'>
                <q-item clickable v-ripple>
                  <q-item-section avatar>
                     <q-avatar :color="rmcqtaker.mock==1?'primary':'info'" text-color="white">
                      <span v-if="rmcqtaker.mock==1">M</span> 
                      <span v-if="rmcqtaker.mock==2">R</span> 
                     </q-avatar>
                  </q-item-section> 
                  <q-item-section>
                    <q-item-label>{{rmcqtaker.tpname}}</q-item-label>
                    <q-item-label caption>{{rmcqtaker.subject}}</q-item-label>
                    <q-item-label caption>{{rmcqtaker.fname}}</q-item-label>
                  </q-item-section>

                  <q-item-section side top>
                    <q-item-label caption>{{ rmcqtaker.tookon }}</q-item-label>
                    <div class="text-orange" v-for="stars in rmcqtaker.attempts">
                      <q-icon name="repeat" /> 
                    </div>
                  </q-item-section>

                </q-item> 
              </q-list>
            </q-tab-panel>
          </q-tab-panels>
        </q-card>
      </div>
 
    </div>
  
  <!-- row 2 end  -->

</q-page>        
</template>
<script>
import { mapState } from "vuex";  
import { mapGetters } from "vuex";
import { mapActions } from "vuex"; 
import axios from "axios";
import { date } from 'quasar' 
 
let timeStamp = Date.now() 
let lastweek = date.formatDate(date.addToDate(timeStamp, { days: -7}),'DD-MM-YYYY') 
let nxtweek = date.formatDate(date.addToDate(timeStamp, { days: 7}),'DD-MM-YYYY') 
let sterday = date.formatDate(date.addToDate(timeStamp, { days: -1}),'DD-MM-YYYY') 
let today = date.formatDate(timeStamp, 'YYYY-MM-DD HH:mm:ss')  //2020-08-22 16:37:57
let aaj = date.formatDate(timeStamp, 'DD-MM-YYYY')  //2020-08-22 16:37:57
export default {
  components: { 
    'single-file-upload' : require('components/File/FileUpload.vue').default,
    'quest-mcq' : require('components/Questions/Mcq.vue').default,
    // 'quest-fib' : require('components/Questions/Fib.vue').default,
    'quest-fib-new' : require('components/Questions/FibNew.vue').default,
    'quest-oeq' : require('components/Questions/Oeq.vue').default,
    'quest-sta' : require('components/Questions/Sta.vue').default,
    'quest-match' : require('components/Questions/Match.vue').default,
    'quest-prepare' : require('components/Questions/PrepareTestPaper.vue').default, 
  },
  mounted(){
     
 //  this.initGroup() 
    //this.initBasicUsers()   
    this.activegroup =   this.mysettings.dashgroupname   
   // let grpid = this.$q.sessionStorage.getItem('gv.grpid')[0];
   // if (  grpid != null ){
     if (this.activegroup ){ 
       let grpid = this.getGroupIdForName(this.activegroup)  
      if ( typeof  grpid == 'undefined'){ 
          this.getGroupUsers()
      }
      else
        this.$q.sessionStorage.set('gv.grpid',grpid )
    }else{
       this.getGroupUsers()
    } 
  },
  computed: {  
    ...mapState("myusers", ['notifystate']),
    ...mapState("mysettings", ['mysettings']),
    ...mapGetters("myusers", ["myusers","loggeduser"]), 
  //  ...mapGetters("mysettings", ['mysettings','getDashGroupName']), 
    ...mapGetters("mygroups", ['mygroups','getGroupIdForName']), 
    tablabel (){ 
      return "1."+this.crqtype+" Bank "   
    },
    arrayUsers() {  
        return Object.values(this.myusers) 
    },
    formatMyDate(str){ 
      return date.formatDate(str,'DD-MM-YYYY')
    }    
  },  
  watch: {
    notifystate(newValue, oldValue) {
     // console.log(`Updating from ${oldValue.status} to ${newValue.status}`);
      if ( this.notifystate.status === 'error' ){
        this.showNotif(
             newValue.msgtext + " Connection Failed. Try again later "  ,
            "report_problem",
            "negative"
        );
      }else if ( this.notifystate.status === 'success' ){
        this.showNotif(
             newValue.msgtext + " "  ,
            "check",
            "success"
        );
      }
    },
    'limit':function(){
      if ( this.limit < 1 ){
        console.log('limit')
        this.limit = 3
      }
    }
  },
  methods: {    
    ...mapActions("myusers", ["initBasicUsers","addUser",]), 
    ...mapActions("mysettings", ["updateSettings"]), 
   // ...mapActions("mygroups", ["initGroup"]), 
     getGroupUsers(){  
      let sgrpid = this.$q.sessionStorage.getItem('gv.grpid')  
      let susrid = this.$q.sessionStorage.getItem('gv.usrid') 
      let goquery = true;
    //  console.log(' getgroupusers ', sgrpid)
      if (sgrpid == 'null' ||  typeof  sgrpid=='undefined' ){
        goquery = false
        //this.showNotif('Groups association seems missing. Relink group', 'red',"negative")
        this.$q
        .dialog({ 
          dark: true,
          title: "Activate Group?",
          message:"Looks like group link is broken. Select a group upon redirect.",
          cancel: true,
          persistent: true
        })
        .onOk(() => { 
              this.$router.push("/group");
        })
        .onCancel(() => {
          this.showNotif("Group Association a must for proper behaviour", "info", "Warning");
        })
        .onDismiss(() => {
          // console.log('I am triggered on both OK and Cancel')
        });

      }
      
      if (goquery) { 
        let dbdata = new FormData();
      dbdata.append("makerid", susrid);
      dbdata.append("grpid", sgrpid);
         axios 
        .post('https://guruttam.com/guruquiz/quizhome/groupusers',
          dbdata
         )
        .then(res => {
          let x = res.data
        //    console.log(' getgroupusers  jsonify ', JSON.stringify(res.data)) 
         if ( res.data[0].status == 0 ) { 
             this.resultarr = []
              this.showNotif( "No matching data " , "info", "warning" );
         }
          else {
              this.resultarr = res.data
            //  this.showNotif( "Group users data fetched" , "info", "info" );   
          }
         })
         .catch(error => { 
        //  if ( error ){
            console.log(error)
        //    console.log(' getgroupusers cacth ', sgrpid)
            this.showNotif( "Network or technical issue. Try later" , "info", "warning" );
         // }
        })       
      }
    },   
    alertTpDelete(tpname,id){
      this.confirmAns("Are your sure to delete "+ tpname +" testpaper ?",id,"Deleteing this testpaper will delete all answers if any as well. OK to Delete or Cance to ingore.")
    },
    confirmAns(msgtitle,id,msg) {         
          this.$q
            .dialog({ 
              dark: true,
              title: msgtitle,
              message:msg,
              cancel: true,
              persistent: true
            })
        .onOk(() => {
            this.deleteTp(id) 
         })
        .onCancel(() => {
          this.showNotif("Deletion ignored", "info", "info");
        })
        .onDismiss(() => {
          // console.log('I am triggered on both OK and Cancel')
        }); 
    },
    deleteTp(id){
        let dbdata = new FormData();
        let usertype = this.$q.sessionStorage.getItem('gv.usertype')  
        dbdata.append("usertype", usertype);
        dbdata.append("tpid", id);
        if ( usertype==1) {
          axios 
          .post('https://guruttam.com/guruquiz/quizhome/deletetp',
            dbdata
          )
          .then( res => {
            let x = res.data  
            if ( x[0].status == 1) {
                this.showNotif( "Testpaper deleted" , "info", "info" ); 
            }
            else{
                this.showNotif( x[0].log , "info", "info" ); 
            }  
          })
          .catch(error => {  
              console.log(error) 
              this.showNotif( "Network or technical issue. Try later" , "info", "warning" ); 
          }) 
        }
    },
    getGroupDoubts(){
        let dbdata = new FormData();
        let sgrpid = this.$q.sessionStorage.getItem('gv.grpid')  
        dbdata.append("grpid", sgrpid);
         axios 
        .post('https://guruttam.com/guruquiz/quizhome/getGroupDoubts',
          dbdata
         )
        .then(res => {
          let x = res.data 
         if ( res.data == false ) { 
             this.rmcqdoubtsarr = []
              this.showNotif( "No matching data " , "info", "warning" );
         }
          else {
              this.rmcqdoubtsarr = res.data
            
            //  this.showNotif( "Group users data fetched" , "info", "info" );   
          }
         })
         .catch(error => { 
        //  if ( error ){
            console.log(error)
        //    console.log(' getgroupusers cacth ', sgrpid)
            this.showNotif( "Network or technical issue. Try later" , "info", "warning" );
         // }
        })     
    },    
 
    getRecentMCQTakers(){
        let dbdata = new FormData();
        let sgrpid = this.$q.sessionStorage.getItem('gv.grpid')  
        dbdata.append("grpid", sgrpid);
         axios 
        .post('https://guruttam.com/guruquiz/quizhome/getrecentmcqtakers',
          dbdata
         )
        .then(res => {
          let x = res.data
        //    console.log(' getgroupusers  jsonify ', JSON.stringify(res.data)) 
         if ( res.data == false ) { 
             this.rmcqtakeresarr = []
              this.showNotif( "No matching data " , "info", "warning" );
         }
          else {
              this.rmcqtakeresarr = res.data
              this.notifycount = res.data.length
            //  this.showNotif( "Group users data fetched" , "info", "info" );   
          }
         })
         .catch(error => { 
        //  if ( error ){
            console.log(error)
        //    console.log(' getgroupusers cacth ', sgrpid)
            this.showNotif( "Network or technical issue. Try later" , "info", "warning" );
         // }
        })     
    },
    clickHistory(){ 
      let sgrpid = this.$q.sessionStorage.getItem('gv.grpid')  
      let susrid = this.$q.sessionStorage.getItem('gv.usrid')      
      
      let dbdata = new FormData();
      dbdata.append("usrid", susrid);
      dbdata.append("grpid", sgrpid);
      dbdata.append("ignoregrpid", 1);
      dbdata.append("sdate", date.formatDate(date.extractDate(this.sdate, 'DD-MM-YYYY'), 'YYYY-MM-DD'));
      dbdata.append("edate", date.formatDate(date.extractDate(this.edate, 'DD-MM-YYYY'), 'YYYY-MM-DD'));
      dbdata.append("clientitme", today); 
   //   console.log(this.sdate, ' ', this.edate) 
      dbdata.append("limit", this.limit);
         axios 
        .post('https://guruttam.com/guruquiz/quizhome/getMakerTestPaper',
          dbdata
         )
        .then(res => {
          let x = res.data
        //    console.log(' getgroupusers  jsonify ', JSON.stringify(res.data)) 
         if ( res.data[0].status == 0 ) { 
             this.tpaper = []
              this.showNotif( "No matching data " , "info", "warning" );
         }
          else {
              this.tpaper = res.data
            //  this.showNotif( "Group users data fetched" , "info", "info" );   
          }
         })
         .catch(error => { 
        //  if ( error ){
            console.log(error)
        //    console.log(' getgroupusers cacth ', sgrpid)
            this.showNotif( "Network or technical issue. Try later" , "info", "warning" );
         // }
        })              
    }, 
    checkQClick(qtype){
     //  this.showNotif( this.mysettings.dashgroupname + " aslask "  , "check", "success" );
      this.crqtype = qtype
    },
    toggleRow(idx){
            let len = Object.keys(this.testpapers).length
            for(let i = 0; i < len ; i++){
              if ( i != idx )
                this.testpapers[i].checked=false 
            }
            this.testpapers[idx].checked=!this.testpapers[idx].checked
            this.testidx = idx
    },
    iconifyq(status){
        
        if ( status == 0 )      // draft
          return 'text-red-3'
        else if (status == 1 )  // review
          return 'text-amber-3'  
        else if ( status == 2 ) // published
          return 'text-blue-3'  
        else
          return 'text-green-3' // taken 
    },
    saveOneUser(){
      //console.log(' One user saved ')
      this.adduser.grpname = this.activegroup;
      this.addUser(this.adduser)
      this.showNotif('Saved','check','green');
      this.adduser.email = '';
      this.adduser.phone = '';
      this.adduser.name = '';
      this.adduser.grpname ='';
    },
    showNotif(msg, type, color) {
      this.$q.notify({
        message: msg,
        position: "top",
        caption: "now",
        icon: type,
        timeout: 500,
        color: color
      });
    }
  },
  data() {
    return {
      sdate: lastweek,
      edate: aaj,
      limit:5,
      tpaper:[],
      resultarr:[],
      rmcqtakeresarr:[],

      rmcqdoubtsarr:[],
      notifycount:0,
      adduser:{
        email:'',
        name:'',
        phone:'',
        type:2
      },
      showgroupusers:false,
      prvwtext:'',
      crqtype:'Multiple Choice',
      activegroup:false,
      slide: 'first',
      testidx:0,
      tabrecent: 'paper',
      tabqpaper: 'question',
      toggleColor:false, 
      
      usrcolumns: [ 
        { name: 'id', label: 'Taker Id', field: 'tusrid',align: 'left',sortable: true, }, 
        { name: 'email', label: 'Email', field: 'email', align: 'left', sortable: true, required:true },  
        { name: 'fname', label: 'Taker', field: 'fname', align: 'left', sortable: true, required:true }, 
        { name: 'grpdate', label: 'Joined', field: 'grpdate', align: 'left', sortable: true, required:true }, 
      ],
      userfilter: '',
      usermode: 'list', 
      pagination: {
          rowsPerPage: 6
      },    
 
      testpapers: [
 

      ]
    }
  }
}
</script>
<style >
.custom-caption {
	text-align: center;
	padding: 12px;
	color: white;
	background-color: rgba(0, 0, 0, 0.3);
}
</style>
