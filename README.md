# doplugins-
temp doplugins
s.usePlugins=true;

var s_doPlugins = function (s) {
  s.AudienceManagement.setup({
    "partner": 'cbsi',
    "containerNSID": 0,
    "uuidCookie": {
      "name":"aam_uuid",
      "days":30
    },
   "visitorService": {namespace:"10D31225525FF5790A490D4D@AdobeOrg"},  
  }); 
  try {
    if (utag_data['userId']) {
      s.visitor.setCustomerIDs({ 
        "other":{ 
            "id":utag_data['userId'], 
            "authState":Visitor.AuthState.AUTHENTICATED 
        }, 
        "cbsihash":utag_data['userHash']
      });
    }
  } catch(err) {
    console.log(err)
  } 
  // AAM SSF Disable Flag for Link Tracking
  //s.contextData['cm.ssf'] = b['cm.ssf'];
  //s.linkTrackVars += ',contextData.cm.ssf';
  //s.useBeacon = true converts all get calls into post
  //This flag helps us to record all the cancelled & failure calls
  s.useBeacon = true;
  s.prop1=(typeof s.eVar1 != 'undefined')?"D=v1":"";
  s.prop2=(typeof s.eVar2 != 'undefined')?"D=v2":"";
  s.prop3=(typeof s.eVar3 != 'undefined')?"D=v3":"";
  s.prop4=(typeof s.eVar4 != 'undefined')?"D=v4":"";
  s.prop5=(typeof s.eVar5 != 'undefined')?"D=v5":"";
  s.prop6=(typeof s.eVar6 != 'undefined')?"D=v6":"";
  s.prop7=(typeof s.eVar7 != 'undefined')?"D=v7":"";
  s.prop8=(typeof s.eVar8 != 'undefined')?"D=v8":"";
  s.prop10=(typeof s.eVar10 != 'undefined')?"D=v10":"";
  s.prop11=(typeof s.eVar11 != 'undefined')?"D=v11":"";
  s.prop12=(typeof s.eVar12 != 'undefined')?"D=v12":"";
  s.prop13=(typeof s.eVar13 != 'undefined')?"D=v13":"";
  s.prop20=(typeof s.eVar20 != 'undefined')?"D=v20":"";
  s.prop21=(typeof s.eVar21 != 'undefined')?"D=v21":"";
  s.prop22=(typeof s.eVar22 != 'undefined')?"D=v22":"";
  s.prop23=(typeof s.eVar23 != 'undefined')?"D=v23":"";
  s.prop24=(typeof s.eVar24 != 'undefined')?"D=v24":"";
  s.prop25=(typeof s.eVar25 != 'undefined')?"D=v25":"";
  s.prop26=(typeof s.eVar26 != 'undefined')?"D=v26":"";
  s.prop30=(typeof s.eVar30 != 'undefined')?"D=v30":"";
  s.prop31=(typeof s.eVar31 != 'undefined')?"D=v31":"";
  s.prop35=(typeof s.eVar35 != 'undefined')?"D=v35":"";
  s.prop44=(typeof s.eVar44 != 'undefined')?"D=v44":"";
  s.prop60=(typeof s.eVar60 != 'undefined')?"D=v60":"";
  s.prop61=(typeof s.eVar61 != 'undefined')?"D=v61":"";
  s.prop62=(typeof s.eVar62 != 'undefined')?"D=v62":"";
  s.prop63=(typeof s.eVar63 != 'undefined')?"D=v63":"";
  s.prop64=(typeof s.eVar64 != 'undefined')?"D=v64":"";
  s.prop65=(typeof s.eVar65 != 'undefined')?"D=v65":"";
  s.prop66=(typeof s.eVar66 != 'undefined')?"D=v66":"";

  
/*if (window.optimizely && typeof window.optimizely.get === 'function' && window.optimizely.get("custom/adobeIntegrator")) {
  window.optimizely.get("custom/adobeIntegrator").assignCampaigns(s);
  window.optimizely.push({
   type: "sendEvents"
  });
}*/
window.optimizely = window.optimizely || [];
window.optlyTracked = false;
if ((window.optimizely) && (typeof window.optimizely.get === 'function') && (window.optimizely.get("custom/adobeIntegrator"))) {
    window.optlyTracked = true;
    window.optimizely.get("custom/adobeIntegrator").assignCampaigns(window.s);
    window.optimizely.push({
        type: "sendEvents"
    });
    window.optimizely.push({
        type: "event",
        eventName: "cbs_adobe_track"
    });
  if (!!s.events) {
    s.events = s.events + ",event68";
  } else {
    s.events = "event68";
  }
}
  //updating rowHeaderPosition for first Header upsell tracking
  if(utag_data.rowHeaderPosition != null && utag_data.rowHeaderPosition == 0){
    s.contextData.rowHeaderPosition = utag_data.rowHeaderPosition.toString();
  }
  //set analytics legacy visitor id into utag data layer obj
  utag_data.legacyAid = s.visitor.getAnalyticsVisitorID();
  
  //set _url
  if(utag_data.isSPA == true && utag_data.isLive == true){
    utag_data._url = utag_data.url;
  }
  else if(utag_data.isSPA == true && utag_data.isLive == false){
    utag_data._url = utag_data.pageUrl;
  }
  else {
    utag_data._url = utag_data._pageUrl;
  }

  // to remove extra prodView being sent on click event:
if(utag_data.page_type != null && utag_data.page_type == "svod_upsell"){
    if(s.linkName != null && s.linkName == "trackClick"){
        if(s.events == "prodView,event19"){
            s.events = "event19";
        }
    }
}
 //remove linkName & linkText on page view calls 
   if(s.linkName != null && s.linkName.toString().indexOf('track') < 0){
      utag_data['link_text'] = '';
      utag_data['link_name'] = '';
     }
  //remove search variables getting persists on trackAction calls
  if(s.linkName != null && s.linkName.toString().indexOf('track') > -1){
    s.contextData.searchEventStart = '';
     }
  //set movieId & movieName
  if(document.location.pathname != null && document.location.pathname.indexOf('/movies/') >-1){
    if(utag_data.videoId != null){
      s.contextData.movieId = utag_data.videoId;
      s. contextData.movieTitle = utag_data.videoTitle;
       }
     }
  //set cm.ssf falg to 1 for kids genre and kids profiles
 if(utag_data && utag_data._genre && utag_data._genre.indexOf('kids') >= 0 || utag_data.userProfileCategory == "KIDS" || utag_data.userProfileCategory == "YOUNGER_KIDS"){
    s.contextData["cm.ssf"] = "1"; 
     utag_data.ssfFlag = "1";
     }
  else {
    s.contextData["cm.ssf"] = "0"; 
    utag_data.ssfFlag = "0";
  }
  //nullify registration success events on non expected pages
  if(s.linkName != null && s.linkName != "trackRegistrationNew" ){
    if(s.contextData.pageType != null && s.contextData.pageType != "svod_signup"){
      if(s.contextData.linkName != null && s.contextData.linkName == "trackRegistrationNew"){
      s.contextData.linkName = '';
      s.contextData.userEventRegistrationSuccess = '';
	 }
       }
    }

 //nullify product variables on skip & Slection button
  if(s.linkName != null && s.linkName == "trackShowPickerSkip"){
     clearProductVariables();
     }
  
  if(s.linkName != null && s.linkName == "trackShowPickerSelection"){
     clearProductVariables();
     }
  
  //clear vars on show picker screen load & picker selection
  if(typeof s != "undefined" && s.contextData.pageType != null && s.contextData.pageType == "svod_show-picker"){
      clearProductVariables();
     }
  if(s.linkName != null && s.linkName == 0){
    if(s.linkType != null && s.linkType == 'e'){
      clearProductVariables(); 
       }
  }
  
  //suppressing purchase trackAction call  
  if(s.linkName != null && s.linkName == "trackPaymentComplete"){
     //s.abort = true;
     }
  //suppressing additional tracking call for trackAppLog from INTL merge
    if(s.linkName != null && s.linkName == "trackAppLog"){
     s.abort = true;
     }
  
  //suppress double firing of page load tags on Sign in page
  if(document.location.pathname == "/account/signin/" && signinFlag == false && s.linkName != null ){
     if(s.linkName.indexOf("track") < 0){
    s.abort = true;
  }
     }
  
  //suppressing trackAction call for keepwatching carousel edit
  if(s.contextData.linkName != "" && s.contextData.linkName == "trackEditKW"){
   // s.abort = true;
     }
   
  //Suppress compiling page pixel
   if(typeof s != "undefined" && s.contextData.pageType != null && s.contextData.pageType == "compiling-page"){
      s.abort = true;
     }
//Resolve purchaseEventBillingStart persistency issue
  if(typeof s != "undefined" && s.linkName != 0 && s.contextData.pageType != "svod_payment" ){
     s.contextData.purchaseEventBillingStart = "";
     }
 
//Resolve switchPlanClick persistency issue
  if(typeof s != "undefined" && s.linkName != 0 && s.contextData.pageType != "svod_switch-plan" ){
     s.contextData.switchPlanClick = "";
     }
  
  //Resolve SearchStart persistency issue
  if(typeof s != "undefined" && s.linkName == 0 && s.contextData.pageType == "search"){
    s.contextData.searchEventStart = "";
     }
  
  //Resolve NFL opt-in tracking persistency issue
 /* This code will not fix the persistency issue
  if(typeof s != "undefined"  && s.linkName != "trackNFLModalView" && s.contextData.linkName == "trackNFLModalView"){
     s.contextData.linkName = "";
     s.contextData.modalMessage = "";
     } */
  
//Kochava Adobe Integration logic
kochavaAdobeInstallEvent();
  
//set signinFlag to false
 signinFlag = false;

  //Supress Vuejs & SPA pages double firing
/*  if(utag_data.isSPA != undefined && utag_data.isSPA == true){
    utag_data.isSPA = false;
    s.abort = true;  
     } */

  
  // set userRegType on Sports Rendezvous Payment Flow
  /* if(s.c_r('sportsRendezvous') != null && s.c_r('sportsRendezvous') != ""){
    if(s.contextData.pageType == "svod_pickaplan_explainer" || s.contextData.pageType == "svod_pickaplan" || s.contextData.pageType == "svod_signup_explainer" || s.contextData.pageType == "svod_signup" || s.contextData.pageType == "svod_payment" || s.contextData.pageType == "svod_complete"){
    s.contextData.userRegType = "cbssports_rendezvous";   
    }
  } */
};

function kochavaAdobeInstallEvent(){
  try{
    adobeMid = s.visitor.getMarketingCloudVisitorID(); 
    adobeAid = s.visitor.getAnalyticsVisitorID(); 
    if(document.location.pathname != "/shows/picker/" && typeof kochava != "undefined"){ 
      if(!kochava.getInstallSent()) {
	kochava.installWithIdentity({marketingcloudvisitorid: adobeMid, adobevisitorid: adobeAid});
      }
    } 
  }
  catch(e){
  }
}

function clearPickAPlanVars() {
  s.contextData.linkName = '';
  s.contextData.PickPlanType = '';
  s.contextData.PickPlanPayment = '';
  s.contextData.PickPlanCTA = '';
  s.contextData.PickPlanSelection = ''; 
  }    

function clearProductVariables() {
  s.events = '';
  s.products = '';
  s.contextData.purchaseProduct = '';
  s.contextData.purchaseProductName = '';
  s.contextData.purchasePaymentMethod = '';
  s.contextData.productOfferPeriod = '';
  s.contextData.purchasePrice = '';
  s.contextData.productPricingPlan = '';
  s.contextData.purchaseCategory = '';
  s.contextData.purchaseOrderId = '';
  s.contextData.purchaseEventOrderComplete = '';
  
}
s.doPlugins = s_doPlugins;
