<template>
    <div>
      <main>
                <div id="myForm" style="padding:5px; height: auto; width: 520px">
                    <iframe id="pay-form"></iframe>
    
                    
    
                </div>
                
            </main>

    
       <h1>{{this.email}}</h1>
    </div>
</template>

<script>
 
export default {
  props: {
    total: Number,
    email: String,
  },

  data() {
    return {
   
    };
  },

 
  mounted() {
    
    this.ScriptRender();
   

  },

  methods: {

     
     

  ScriptRender(){
          
           async function HMAC(key, message){
                  const g = str => new Uint8Array([...unescape(encodeURIComponent(str))].map(c => c.charCodeAt(0))),
                  k = g(key),
                  m = g(message),
                  c = await crypto.subtle.importKey('raw', k, { name: 'HMAC', hash: 'SHA-256' },true, ['sign']),
                  s = await crypto.subtle.sign('HMAC', c, m);
                  return [...new Uint8Array(s)].map(b => b.toString(16).padStart(2, '0')).join('');
                }
    
                /* Values that will be needed for generating the HMAC */
                let host_domain = "http://127.0.0.1:8000";
                //let qor_form_id = "frm_e67ba9b701de11ed87940aac2e024c3e"; // test form_id from a saved embedded form template
                let qor_form_id = "frm_b903c7670f6b11ed9d150aac2e024c3e"; // test form_id from a saved embedded form template - clean eatz
                let qor_form_id2 = "frm_6adbb4c30e5f11ed9d150aac2e024c3e"; // test form_id from a saved embedded form template
                let qor_app_key = "T6554252567241061980"; // test app-key
                let qor_client_key = "01dffeb784c64d098c8c691ea589eb82"; // test client-key
                let qor_mid = "887728203"; // test MID
    
                const payfields = {};
    
                payfields["width"] = '520'+'px'; // sets payment form DIV width and passed along to payment form -> 400px is single column, 520px (+) grid rendered
    
                document.getElementById("myForm").style.width = payfields["width"];
    
                const params = (new URL(location)).searchParams;
                
                switch(params.get('option')) {
                    case '1':
    
                        /** use this form to create a payment intent and tokenize the account to be used in your own payment attempt ***/
    
                        payfields["timestamp"] = Math.floor(Date.now() / 1000); // Current Unix timestamp -> used for auto-reload
                        payfields["payment-intent"] = 1; 
                        payfields["domain"] = host_domain; // Domain of the website that will host this embeddedpayment form
                        payfields["app-key"] = qor_app_key;
                        payfields["client-key"] = qor_client_key;
                        payfields["mid"] = qor_mid;
                        payfields["include-cardholder"] = 1; // turn cardholder field on / off
                        payfields["include-street"] = 0;  // turn steet field on / off
                        payfields["include-zip"] = 1; // turn zip field on / off -> used for AVS
                        payfields["button-submit"] = 1;
                        payfields["button-text"] = "Place Order (intent)"; // set the submit button text
                        //payfields["profile-id"] = '12345'; // tis is yor profile / customer id that we can assign to the token
    
                        break;
    
                    case '2':
    
                        /** -- OR -- **/
                        /** use this form to create a payment token to be used in your own payment attempt or simply save a payment method and store that token for your customer ***/
                        
                        payfields["timestamp"] = Math.floor(Date.now() / 1000); // Current Unix timestamp -> used for auto-reload
                        payfields["payment-token"] = 1; 
                        payfields["domain"] = host_domain; // Domain of the website that will host this embeddedpayment form
                        payfields["app-key"] = qor_app_key;
                        payfields["client-key"] = qor_client_key;
                        payfields["mid"] = qor_mid;
                        payfields["include-cardholder"] = 1; // turn cardholder field on / off
                        payfields["include-street"] = 0;  // turn steet field on / off
                        payfields["include-zip"] = 1; // turn zip field on / off -> used for AVS
                        payfields["button-submit"] = 1;
                        payfields["button-text"] = "Create Payment Token"; // set the submit button text
                        //payfields["profile-id"] = '12345'; // tis is yor profile / customer id that we can assign to the token
    
                        break;
    
                    case '3':
    
                        /** -- OR -- **/
                        /*** send a form ID for a saved form template built from the assorted option of attributes ***/
    
                        // JSON of attributes saved in the form template (DB)
                        /*
                        {
                            "payment-intent": 0,
                            "app-key": "T6554252567241061980",
                            "client-key": "01dffeb784c64d098c8c691ea589eb82",
                            "domain": "https://secure.qorcommerce.io",
                            "mid": "887728203",
                            "use3DS": 0,
                            "css-url": "https://secure.qorcommerce.io/css/standard.css",
                            "include-cardholder": 1,
                            "include-street": 0,
                            "include-zip": 1,
                            "button-submit": 1,
                            "button-text": "Place Order",
                            "auto-reload": 0,
                            "store-card": 0,
                            "include-store-card": 1,
                            "store-card-text": "Store for later use",
                            "required-fields": "cardholdername,account,expdate,cv,zip"
                        }
                        */
    
                        payfields["timestamp"] = Math.floor(Date.now() / 1000); // Current Unix timestamp -> used for auto-reload
                        payfields["form_id"] = qor_form_id; 
                        payfields["domain"] = host_domain; // id this is sent this overrides the saved host domain
                        payfields["app-key"] = qor_app_key;
                        payfields["client-key"] = qor_client_key;
                        payfields["amount"] = "10.10"; // amount of order or shopping cart to be charged
                        payfields["orderid"] = "oid-" + Math.floor(Math.random() * 100000) + 1000;; // unique order id - REPLACE with our oredr id
                        //payfields["profile-id"] = '12345'; // tis is yor profile / customer id that we can assign to the order
    
                        break;
    
                    case '4':
    
                        /** -- OR -- **/
                        /*** send everything to build an embedded form ***/
                    
                        payfields["timestamp"] = Math.floor(Date.now() / 1000); // Current Unix timestamp -> used for auto-reload
                        payfields["domain"] = host_domain; // Domain of the website that will host this embeddedpayment form
                        payfields["app-key"] = qor_app_key;
                        payfields["client-key"] = qor_client_key;
                        payfields["mid"] = qor_mid;
                        //payfields["profile-id"] = '12345'; // tis is yor profile / customer id that we can assign to the order
    
                        //$payfields["background-color"] = "#e7f0ff"; // a color eg. red -or- the hex eg. #e7f0ff
    
                        payfields["amount"] = "10.10"; // amount of order or shopping cart to be charged
                        payfields["orderid"] = "oid-" + Math.floor(Math.random() * 100000) + 1000;; // unique order id - REPLACE with our oredr id
    
                        //payfields["account"] = "131942$U60MLwx2"; // send a tokenized account to be charged (???? needs to be encrypted)
    
                        payfields["use3DS"] = 0; // turn on 3D secure usage
    
                        payfields["css-url"] = host_domain + "/css/standard.css";
                        //payfields["css-url"] = host_domain + "/css/modern.css";
                        //payfields["css-url"] = host_domain + "/css/floating.css"; // not done yet
    
                        //payfields["expdate-format"] = "separate"; // will also render exp month/year MM/YYYY - separate is default
                        //payfields["expdate-format"] = "merged"; // will also render exp month/year MM/YYYY - separate is default
                        payfields["include-cardholder"] = 1; // turn cardholder field on / off
                        //payfields["include-street"] = 1;  // turn steet field on / off
                        payfields["include-zip"] = 1; // turn zip field on / off -> used for AVS
    
                        //payfields["auto-reload"] = 1; // not done yet -> used for AVS
    
                        //payfields["store-card"] = 1; // store card for later use (manual push) - if this is set then include-store-card is by passed
                        //payfields["include-store-card"] = 1; // show "Store for later use" checkbox
                        //payfields["store-card-text"] = "Store for later use"; // set the store card text on the form
    
                        payfields["button-submit"] = 0;
                        payfields["button-text"] = "Place Order"; // set the submit button text
    
                        break;
    
                    case '5':
    
                        /** use this form to send a payment token and verify the CC CVV for the payment attempt ***/
    
                        payfields["timestamp"] = Math.floor(Date.now() / 1000); // Current Unix timestamp -> used for auto-reload
                        payfields["payment-cvv"] = 1; 
                        payfields["domain"] = host_domain; // Domain of the website that will host this embeddedpayment form
                        payfields["app-key"] = qor_app_key;
                        payfields["client-key"] = qor_client_key;
                        payfields["mid"] = qor_mid;
                        payfields["account"] = "131942$U60MLwx2"; // send a tokenized account to be charged (???? needs to be encrypted)
                        payfields["amount"] = "10.10"; // amount of order or shopping cart to be charged
                        payfields["orderid"] = "oid-" + Math.floor(Math.random() * 100000) + 1000;; // unique order id - REPLACE with our oredr id
                        payfields["button-submit"] = 1;
                        payfields["button-text"] = "Place Order (verify CVV)"; // set the submit button text
    
                        break;
    
                    case '6':
    
                        /** use this form to create a payment intent and tokenize the account to be used in your own payment attempt ***/
    
                        payfields["timestamp"] = Math.floor(Date.now() / 1000); // Current Unix timestamp -> used for auto-reload
                        payfields["cctoken-update"] = 1; 
                        payfields["domain"] = host_domain; // Domain of the website that will host this embeddedpayment form
                        payfields["app-key"] = qor_app_key;
                        payfields["client-key"] = qor_client_key;
                        payfields["account"] = "131942$U60MLwx2"; // send a tokenized account to be updated
                        payfields["button-submit"] = 1;
                        payfields["button-text"] = "Update Token"; // set the submit button text
    
                        break;
    
                    case '7':
    
                        /*** Use the modern CSS example **/
                        /*** send a form ID for a saved form template built from the assorted option of attributes ***/					
    
                        payfields["timestamp"] = Math.floor(Date.now() / 1000); // Current Unix timestamp -> used for auto-reload
                        payfields["form_id"] = qor_form_id2; 
                        payfields["css-url"] = host_domain + "/css/modern.css";
                        payfields["domain"] = host_domain; // id this is sent this overrides the saved host domain
                        payfields["app-key"] = qor_app_key;
                        payfields["client-key"] = qor_client_key;
                        payfields["amount"] = "10.10"; // amount of order or shopping cart to be charged
                        payfields["orderid"] = "oid-" + Math.floor(Math.random() * 100000) + 1000;; // unique order id - REPLACE with our oredr id
    
                        break;
    
                    default:
    
                        payfields["timestamp"] = Math.floor(Date.now() / 1000); // Current Unix timestamp -> used for auto-reload
                        payfields["form_id"] = qor_form_id; 
                        payfields["domain"] = host_domain; // id this is sent this overrides the saved host domain
                        payfields["app-key"] = qor_app_key;
                        payfields["client-key"] = qor_client_key;
                        payfields["amount"] = "10.10"; // amount of order or shopping cart to be charged
                        payfields["orderid"] = "oid-" + Math.floor(Math.random() * 100000) + 1000;; // unique order id - REPLACE with our oredr id
    
                }
    
    
                /* Response onSuccess INTENT:
    
                {
                    "status":"approved",
                    "code":"GW00",
                    "message":"Get token was processed successfully (existing token).",
                    "last_4":"1319",
                    "token":"t:131942$U60MLwx2"  // temporary token
                }
    
                /* Response onSuccess GET TOKEN:
    
                {
                    "status":"approved",
                    "code":"GW00",
                    "message":"Get token was processed successfully (existing token).",
                    "last_4":"1319",
                    "token":"131942$U60MLwx2"
                }
    
                /* Response onSuccess PAYMENT:
    
                {
                    "status":"approved",
                    "code":"GW00",
                    "message":"Sale processed successfully.",
                    "transaction_date":"2022-07-14 08:46:31",
                    "transaction_id":"219445991395050",
                    "order_id":"oid-2008028340",
                    "last_4":"1319",
                    "brand":"visa",
                    "authcode":"222625",
                    "amount_approved":"20.10",
                    "store_card": 0,
                    "isGC":"no"
                }
    
                /* Response onSuccess w/ token PAYMENT:
    
                {
                    "status":"approved",
                    "code":"GW00",
                    "message":"Sale processed successfully.",
                    "transaction_date":"2022-07-14 08:46:31",
                    "transaction_id":"219445991395050",
                    "order_id":"oid-2008028340",
                    "last_4":"1319",
                    "brand":"visa",
                    "authcode":"222625",
                    "amount_approved":"20.10",
                    "store_card":1,
                    "token":"131942$U60MLwx2",
                    "token_last4":"1319",
                    "token_exp_m":"03",
                    "token_exp_y":"24",
                    "token_brand":"visa",
                    "isGC":"no"
                }
    
                /* Response onError PAYMENT:
    
                {
                    "status":"declined",
                    "code":"GW97",
                    "message":"Sale was not processed. (97: CVV2\/CID error)",
                    "order_id":"oid-153098432",
                    "store_card": 0,
                    "isGC":"no"
                }
    
                */
    
                let data_to_hash = Object.values(payfields).join(''); // implode values only
    
                // hash and build hmac
                HMAC(qor_client_key, data_to_hash)
                     .then(function(hash) { 
                         document.getElementById("pay-form").setAttribute("data-hmac-hmacsha256", hash); 
    
                         for (const [key, value] of Object.entries(payfields)) {
                             document.getElementById("pay-form").setAttribute("data-hmac-"+key, value); 
                        }
    
                         var paymentForm = new QorPaymentForm( "pay-form","https://secure.qorcommerce.io");
    
                        /***
                            * define your own submit button 
                            * When generating the iframe, send the attribute button-submit = 0.
                                This will hide the payment forms default submit button.
                            • to trigger a submit of the payment form, use the paymentForm object's
                                submitPayment method
                        ***/
                        
                        var mySubmitButton = document.getElementById("my-submit-button");
                        mySubmitButton.addEventListener("click", function() {
                                paymentForm.submitPayment();
                            });
                        
    
                        paymentForm.onSubmit(function(response) {
                            if (response.code === 'approved') {
                                alert( "Received an approval: " + response.onSuccess );
                                // record the transaction, save the token if needed and finish order/sale
                                console.log(response.onSuccess);
                                window.location.assign("thank-you.html");
                            } else {
                                alert( "Received a decline: " + response.onError );
                                console.log(response.onError);
                                paymentForm.enableSubmitButton();
                            }
                        });
    
                        paymentForm.request();
                     } );


    },
  },
};
</script>

<style>
div#myForm {
    width: 100% !important;
}
iframe {
	width: 100%;
	border: none;
	margin-top: 1rem;
	padding: 0;
	height: 342px;
}
button {
	padding: .75rem 1rem;
	font-size: 1rem;
	background-color: #DDD;
	border: none;
	cursor: pointer;
}

</style>