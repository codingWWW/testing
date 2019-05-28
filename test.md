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
public function facebook_login(Request $request){

$data=array();

        //check if email not empty

        if($request->input('email')!=''){

            $id=addslashes($request->input('id'));

            $name=addslashes($request->input('name'));

            $email=addslashes($request->input('email'));

            

            $result=DB::select("SELECT id, payment FROM users WHERE email='$email' LIMIT 1");

            if(count($result)==1){

                $result=collect($result)->first();

                

                $request->session()->put('id',$result->id);

                $data['success']=1;

            }

            else {

	      DB::insert(“INSERT INTO users (name, email, facebook_id, reg_on) VALUES (‘$name’, ‘$email’, ’$id’, NOW())”);

	      $id = DB::getPdo()->lastInsertId();

                $request->session()->put('id',$result->id);

                $data['success']=1;

                 }

            return response()->json($data);

        }

}
```
