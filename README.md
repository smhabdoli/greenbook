greenbook
=========

Following treehouse tutorials on how to develop a social networking site with rails 4

## Issue 1
In Chapter 3, Jason talks about `attr_accessible` fields which have been depricated now. So I'll have to add those in one of two ways according to [devise's github](https://github.com/plataformatec/devise):

1. Add a shorthand if only going to be using one devise model as follows:

	```ruby
	class ApplicationController < ActionController::Base
		before_filter :configure_permitted_parameters, if: :devise_controller?

		protected

		def configure_permitted_parameters
			devise_parameter_sanitizer.for(:sign_up) << :username
		end
	end
	```

2. Derive a Parameter Sanitizer per model class from Devise::ParameterSanitizer:

	```ruby
	class User::ParameterSanitizer < Devise::ParameterSanitizer
		def sign_in
			default_params.permit(:username, :email)
		end
	end
	```

	and then modify the controller as follows:

	```ruby
	class ApplicationController < ActionController::Base
		protected

		def devise_parameter_sanitizer
			if resource_class == User
				User::ParameterSanitizer.new(User, :user, params)
			else
				super # Use the default one
			end
		end
	end
	```

I'll read more about this later.