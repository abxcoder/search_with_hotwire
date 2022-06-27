# source : https://www.youtube.com/watch?v=PfCU0Nni8fI  15:43

# rails new search4

# rails g scaffold Movie title:string

# rails db:migrate

# routes.rb
root "movies#index"

# app -> views -> layouts -> application
<link rel="stylesheet" href="https://cdn.simplecss.org/simple.min.css">


# seed.rb
50.times.do
    Movie.create(title: Faker::Movie.unique.title)
end

# Movie.rb
validates:title, presence: true, uniqueness: true

# rails db:seed

# routes.rb
  resources :movies do
    collection do
      post:search
    end
  
  
# app -> views -> movies -> index.html.erb
<% form_with url: search_movies_path, method: :post do |form| %>
  <%= form.search_field :title_search %>
<% end %>
<div id="search_results" >results</div>

# movies_controller.rb
  def search
    # params[:title_search]
    respond_to do |format|
      format.turbo_stream do
        render turbo_stream: turbo_stream.update("search_results", params[:title_search])
      end
    end 
  end