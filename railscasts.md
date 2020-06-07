## Episode 1, Caching with Instance Variables
```
def current_user
  @current_user ||= User.find(session[:user_id])
end
```

## Episode 2, Dynamic find_by Methods
```
def incomplete
  @tasks = Task.find_all_by_complete(false)
end

def last_incomplete
  @task = Task.find_by_complete(false, :order => 'created_at DESC')
end
```


## Episode 3, Find Through Association
```
def show
  @project = Project.find(params[:id])
  @tasks = @project.tasks.find_all_by_complete(false)
end
```

## Episode 4, Move Find into Model
```
# tasks_controller.rb
def index
  @tasks = Task.find_incomplete
end
```
```
# models/task.rb
def self.find_incomplete
  find_all_by_complete(false, :order => 'created_at DESC')
end
```
```
# projects_controller.rb
def show
  @project = Project.find(params[:id])
  @tasks = @project.tasks.find_incomplete
end
```


## Episode 5, Using with_scope
```
# task.rb
class Task < ActiveRecord::Base
  belongs_to :project

  def self.find_incomplete(options = {})
    with_scope :find => options do
      find_all_by_complete(false, :order => 'created_at DESC')
    end
  end
end
```
```
# tasks_controller.rb
@tasks = @project.tasks.find_incomplete :limit => 20
```
