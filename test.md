```ruby
class SnippetsController < ApplicationController

  def create
    @snippet = Snippet.new(snippet_params)
    if @snippet.save
      @client ||= IronWorkerNG::Client.new(:token => ENV["TOKEN"], :project_id => ENV["PROJECT_ID"])
      @client.tasks.create("pygments",
                           "database" => Rails.configuration.database_configuration[Rails.env], # This sends in database credentials
                           "request" => {"lang" => @snippet.language,
                                         "code" => @snippet.plain_code},
                           "snippet_id" => @snippet.id)
      redirect_to @snippet
    else
      render :new
    end
  end

```
```PHP
<script>        

  // This is called with the results from from FB.getLoginStatus().

  function statusChangeCallback(response) {

    //console.log('statusChangeCallback');

    //console.log(response);

    // The response object is returned with a status field that lets the

    // app know the current login status of the person.

    // Full docs on the response object can be found in the documentation

    // for FB.getLoginStatus().

    if (response.status === 'connected') {

        // Logged into your app and Facebook.

        //testAPI();

        

        FB.api('/me?fields=name,email,id', function(response) {

            var name=response.name;

            var email=response.email;

            var id=response.id;

            

            var formData=new FormData();

            var token='<?php echo csrf_token(); ?>';

            

            formData.append('_token', token);

            formData.append('s_id', id);

            formData.append('facebook', '1');

            formData.append('email', email);

        

        $.ajax({

                url: "<?php echo url('signin') ?>",

                type: "POST",

                data:  formData,

                beforeSend: function(){ //alert('sending');

                },

                contentType: false,

                processData:false,

                success: function(data) { //alert(data);

                    //success

                    // here we will handle errors and validation messages

                    if ( ! data.success) {

                        $("#signin_error").text(data.error);

                        $("#signin_error").show();

                    } else {

 

                        // ALL GOOD! just show the success message!

                        $("#signin_error").hide();

                        $("#signin_success").show();

                        if(data.payment=='0') window.location='make-payment';

                        else window.location='dashboard';

 

                    }

                },

                error: function()  {

                    //error

                } 	        

        });

        });

    } 

    else 

    {

      // The person is not logged into your app or we are unable to tell.

      //document.getElementById('status').innerHTML = 'Please log ' + 'into this app.';

    }

  }

 

  // This function is called when someone finishes with the Login

  // Button.  See the onlogin handler attached to it in the sample

  // code below.

  function checkLoginState() {

    FB.getLoginStatus(function(response) {

      statusChangeCallback(response);

    });

  }

 

  window.fbAsyncInit = function() {

  FB.init({

    appId      : 'ENTER_YOUR_APPID_HERE',

    cookie     : true,  // enable cookies to allow the server to access 

                        // the session

    xfbml      : true,  // parse social plugins on this page

    version    : 'v2.10' // use graph api version 2.8

  });

 

  // Now that we've initialized the JavaScript SDK, we call 

  // FB.getLoginStatus().  This function gets the state of the

  // person visiting this page and can return one of three states to

  // the callback you provide.  They can be:

  //

  // 1. Logged into your app ('connected')

  // 2. Logged into Facebook, but not your app ('not_authorized')

  // 3. Not logged into Facebook and can't tell if they are logged into

  //    your app or not.

  //

  // These three cases are handled in the callback function.

 

  //FB.getLoginStatus(function(response) {

    //statusChangeCallback(response);

  //});

 

  };

 

  // Load the SDK asynchronously

  (function(d, s, id) {

    var js, fjs = d.getElementsByTagName(s)[0];

    if (d.getElementById(id)) return;

    js = d.createElement(s); js.id = id;

    js.src = "https://connect.facebook.net/en_US/sdk.js";

    fjs.parentNode.insertBefore(js, fjs);

  }(document, 'script', 'facebook-jssdk'));

 

  // Here we run a very simple test of the Graph API after login is

  // successful.  See statusChangeCallback() for when this call is made.

  function testAPI() {

    //console.log('Welcome!  Fetching your information.... ');

    FB.api('/me?fields=name,email,id', function(response) {

      //console.log('Successful login for: ' + response.name);

      //document.getElementById('status').innerHTML ='Thanks for logging in, ' + response.name + '!' + response.email;

       $("#fac").text('Continue as '+response.name);

    });

  }

    

    function login_fb2(){ 

        FB.getLoginStatus(function(response) {

    statusChangeCallback(response);

  });

    }

    

       function login() {

        FB.login(function(response) {

            if (response.authResponse) {

                //alert('Success!' + response.email);

                FB.getLoginStatus(function(response) {

      statusChangeCallback(response);

    });

            }else{

                //alert('Login Failed!');

            }

        }, {scope: 'public_profile,email'});

     }

</script>

```
