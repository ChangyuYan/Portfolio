### Virtual Env
- `source myvenv/bin/activate`
- `deactivate`

--------------------

### Django Apps
- `python manage.py startapp [app name]`

### Models
- Model for a specific app
- Go to models.py and create python class
- Example:    
	`
	class Job(models.Model):
	    image = models.ImageField(upload_to='images/') 
	    summary = models.CharField(max_length=200)
	`
- Then go to settings.py, add '[app name].apps.[Appname]Config' (auto-created by apps.py) to `INSTALLED_APPS`

- At the end of settings.py, add:    
	`
	MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
	MEDIA_URL = '/media/'
	`
- After creating a model, remember to make migrations

### Creating Admin Accounts
- ` python manage.py createsuperuser`
- In order for apps to show up on the admin page, go to jobs/admin.py:    
	`
	from .models import Job
	admin.site.register(Job)
	`
- Now it would be shown on the admin page

- - When clicking on image, not shown. To fix this, add
	`
	from django.conf import settings
	from django.conf.urls.static import static
	
	.... + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
	` 
	in main urls.py

--------------------
### Homepage
- Say we want to show all the jobs on the homepage
- In jobs/views.py:    
	`
	def home(request):
	    return render(request, 'jobs/home.html')
	`
- In urls.py:    
	`
	import jobs.views
	...
	path('', jobs.views.home),
	`
- In jobs, create templates/jobs/home.html


### Show jobs on homepage
- To get access from the database:
	1. Go to jobs/views.py, add:
		`
		from .models import Job 
		def home(request):
		    jobs = Job.objects
		    return render(request, 'jobs/home.html', {'jobs':jobs})
		`

### Static Files

- In portfolio folder, create static folder to hold all the static files
- In settings.py, add 
	`
	STATICFILES_DIRS = [
	    os.path.join(BASE_DIR, 'portfolio/static/')
	]
	STATIC_ROOT = os.path.join(BASE_DIR, 'static')
	`
- `$ python manage.py collectstatic`
- In home.html, head, add `{% load staticfiles %}`
- Example:  <a class="nav-item nav-link" href="{% static 'resume.pdf' %}">Resume</a>

--------------------
### Other Tips
- When creating a website, always helpful to draw a sketch first