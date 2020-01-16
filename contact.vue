<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loading-spinner v-if="!dataLoaded"></loading-spinner>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
                <div class="inside_page_header" v-if="pageBanner" v-bind:style="{ background: 'linear-gradient(0deg, rgba(0,0,0,0.2), rgba(0,0,0,0.2)), url(' + pageBanner.image_url + ') center center' }">
                    <div class="main_container position_relative">
                        <h2>Contact Us</h2>
                    </div>
                </div>
                <div class="main_container">
                    <div class="row">
                        <div class="col-md-12">
                            <breadcrumb></breadcrumb>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12">
                            <!--<div v-if="main" class="margin_60" v-html="main.body"></div>-->
                            <!--<p class="margin_60">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis ac egestas nisl. Cras odio massa, tincidunt ut faucibus in, egestas non nisl. Morbi vel nibh metus. In quis est eget risus semper facilisis. Sed in felis vel lorem consectetur convallis. Aliquam fringilla facilisis ipsum et sagittis. Maecenas rutrum urna id efficitur ultrices. Duis porttitor, ante quis consectetur aliquet, elit massa dignissim ex, non luctus lacus dolor quis ipsum.</p>-->
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-4">
                            <div v-if="contactInfo" class="margin_60 padding_60" v-html="contactInfo.body"></div>    
                        </div>
                        <div class="col-md-8">
                            <transition name="fadeIn">
                                <div>
                                    <div id="send_contact_success" class="alert alert-success" role="alert" v-show="formSuccess">
                                        <span class="glyphicon glyphicon-ok" aria-hidden="true"></span>
                                        <span class="sr-only">Success</span>
                                        Thank you for contacting us. A member from our team will contact you shortly.
                                    </div>
                                    <div id="send_contact_error" class="alert alert-danger" role="alert" v-show="formError">
                                        <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span>
                                        <span class="sr-only">Error:</span>
                                        There was an error when trying to submit your request. Please try again later.
                                    </div>
                                </div>
                            </transition>
                            <form id="contact_form" class="form-horizontal clearfix" action="form-submit" v-on:submit.prevent="validateBeforeSubmit">
                                <div class="row">
                                    <div class="col-xs-12 col-md-6" :class="{'has-error': errors.has('name')}">
                                        <label class="accessibility" for="name">Name</label>
                                        <input id="name" v-model="form_data.name" v-validate="'required|alpha_spaces'" class="form-control" :class="{'input': true}" name="name" type="text" data-vv-delay="1000" placeholder="Name *">
                                        <span v-show="errors.has('name')" class="form-control-feedback">{{ errors.first('name') }}</span>
                                    </div>
                                    <div class="col-xs-12 col-md-6" :class="{'has-error': errors.has('email')}">
                                        <label class="accessibility" for="email">Email</label>
                                        <input id="email" v-model="form_data.email" v-validate="'required|email'" class="form-control" :class="{'input': true}" name="email" type="email" data-vv-delay="1000" placeholder="Email *">
                                        <span v-show="errors.has('email')" class="form-control-feedback">{{ errors.first('email') }}</span>
                                    </div>
                                </div>
                                <div class="row">
                                    <div class="col-xs-12 col-md-6" :class="{'has-error': errors.has('phone')}">
                                        <label class="accessibility" for="phone">Phone</label>
                                        <input id="phone" v-model="form_data.phone" v-validate="'required:true'" class="form-control" :class="{'input': true}" name="phone" type="text" data-vv-delay="1000" placeholder="Phone *">
                                        <span v-show="errors.has('phone')" class="form-control-feedback">{{ errors.first('phone') }}</span>
                                    </div>
                                    <div class="col-xs-12 col-md-6" :class="{'has-error': errors.has('subject')}">
                                        <label class="accessibility" for="subject">Subject</label>
                                        <input id="subject" v-model="form_data.subject" v-validate="'required:true'" class="form-control" :class="{'input': true}" name="subject" type="text" data-vv-delay="1000" placeholder="Subject *">
                                        <span v-show="errors.has('subject')" class="form-control-feedback">{{ errors.first('subject') }}</span>
                                    </div>
                                </div>
                                <div class="row">
                                    <div class="col-xs-12" :class="{'has-error': errors.has('message')}">
                                        <label class="accessibility" for="message">Message</label>
                                        <textarea id="message" v-model="form_data.message" v-validate="'required:true'" class="form-control" :class="{'input': true}" name="message" type="text" data-vv-delay="1000" placeholder="Message *"></textarea>
                                        <span v-show="errors.has('message')" class="form-control-feedback">{{ errors.first('message') }}</span>
                                    </div>
                                </div>
                                <div class="row margin_60">
                                    <div class="col-xs-12">
                                        <button class="submit animated_btn" type="submit" :disabled="formSuccess">Submit</button>
                                    </div>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>
<script>
var _extends=Object.assign||function(e){for(var t=1;t<arguments.length;t++){var a=arguments[t];for(var r in a)Object.prototype.hasOwnProperty.call(a,r)&&(e[r]=a[r])}return e};define(["Vue","vuex","vue-meta","vee-validate"],function(e,t,a,r){return e.use(a),e.use(r),e.component("contact-component",{template:template,data:function(){return{dataLoaded:!1,pageBanner:null,main:null,contactInfo:null,form_data:{},loginPending:null,formSuccess:!1,formError:!1,time:new Date}},created:function(){var e=this;this.loadData().then(function(t){var a=e.findRepoByName("Contact Us Banner").images;e.pageBanner=null!=a?a[0]:{image_url:"//codecloud.cdn.speedyrails.net/sites/5b71e1e76e6f6411c4060000/image/jpeg/1529532304000/insidebanner2.jpg"},e.main=t[0].data,e.contactInfo=t[0].data.subpages[0],e.dataLoaded=!0})},computed:_extends({},t.mapGetters(["property","findRepoByName"])),methods:{loadData:function(){var e;return regeneratorRuntime.async(function(t){for(;;)switch(t.prev=t.next){case 0:return this.property.mm_host=this.property.mm_host.replace("http:",""),t.prev=1,t.next=4,regeneratorRuntime.awrap(Promise.all([this.$store.dispatch("LOAD_PAGE_DATA",{url:this.property.mm_host+"/pages/costamesa-contact-us.json"}),this.$store.dispatch("getData","repos")]));case 4:return e=t.sent,t.abrupt("return",e);case 8:t.prev=8,t.t0=t["catch"](1),console.log("Error loading data: "+t.t0.message);case 11:case"end":return t.stop()}},null,this,[[1,8]])},validateBeforeSubmit:function(){var e=this;this.$validator.validateAll().then(function(t){if(t){{e.errors}send_data={},send_data.form_data=JSON.stringify(e.serializeObject(e.form_data)),e.$store.dispatch("CONTACT_US",send_data).then(function(){e.formSuccess=!0})["catch"](function(t){try{401==t.response.status?(console.log("Data load error: "+t.message),e.formError=!0):(console.log("Data load error: "+t.message),e.formError=!0)}catch(a){console.log("Data load error: "+t.message),e.formError=!0}})}})},serializeObject:function(e){var t=[];return _.forEach(e,function(e,a){var r={};r.name=a,r.value=e,t.push(r)}),t}}})});
</script>