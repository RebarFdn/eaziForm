# eaziForm 

### Generate Simple and Secure Html forms from Pydantic models.

eaziForm&reg; enables the creation of simple tailwindcss styled Html web forms from basic or nested pydantic models . 

## Installation
Using Github Repository:

	pip install git+git:https://github.com/RebarFdn/easiform
	
## Dependencies

eaziForm&reg; requires python3.10 or higher and pydantic.<br>

**Css**<br> 
Styling is provided by pre loaded tailwindcss stylesheet.<br>

**Icons**<br>
> easyForm&reg; uses fontawesome icons if provided in the application's static resources installed in a folder called fontawesome


# Usage

**Basic Models**
>  # /models.py

	from pydantic import Field, field_validator
	from eaziform import FormModel
	
	
	# Basic  UserModel	
	class UserModel(FormModel):
		name: str = Field(default=None, min_length=3)
		age: int = 0

		@field_validator('age')
		@classmethod
		def age_must_be_over_18(cls, value, values):
		    if value <=18:
		        raise ValueError("age must be over 18.")
		    return value


**Application**<br>
	
**starlette**
>  # /route.py

	from starlette.requests import Request
	from starlette.responses import HTMLResponse
	from decoRouter import Router
	from models import UserModel
	
	
	router = Router()
	
	@router.post('/register')
	@router.get('/register')
	async def Register(request:Request):
		'''  '''
		if request.method == 'POST':
		    form = await request.form() 
		    model = await UserModel().validateForm(request_data=form, schema=MyForm, post='/register')
		    return HTMLResponse(model)
		    
		    
		else:		   
		    form = UserModel().data_form()
		    html_form = UserModel().html_form(post='/register', form=form)
		    return HTMLResponse(html_form)



# Features

1.  Automatic Html form generation.

 - Numeric Fields
 - Text Fields
 - Textarea Fields
 - Checkbox Fields
 - Select Fields
 - Range Fields
 
  
2.  Field validation using pydantic validation mechanism

A GET request to route '/register' would yield up the following form.
	
![alt text](file:///home/ian/eaziForm/imgs/ezForm-basic-input.png
)

eaziForm&reg; automatically validates input data by a POST request using default or provided Htmx enpoints and target attributes.

![alt text](file:///home/ian/eaziForm/imgs/ezForm-basic-validation.png)


Valid form sumission returns a JSON object or html insert.

![alt text](file:///home/ian/eaziForm/imgs/ezForm-basic-result.png)


# Advanced Usage
