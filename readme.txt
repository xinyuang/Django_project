pip install djangorestframework
pip install django
django-admin startproject webAPI
python manage.py startapp webapp 

nano webAPI/settings.py
	INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'webapp'
]

nano webapp/models.py
	class employees(models.Model):
    	    firstname = models.CharField(max_length=10)
   	    lastname = models.CharField(max_length=10)
    	    emp_id = models.IntegerField()

        def __str__(self):
            return self.firstname

nano webapp/admin.py
	from . models import employees
	admin.site.register(employees)

python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser

python manage.py runserver

nano webapp/serializers.py
    from rest_framework import serializers
    from . models import employees

    class employeesSerializer(serializers.ModelSerializer):
        class Meta:
            model = employees
            #fields = ('firstname','lastname')
            fields = '__all__'

nano webapp/views.py
    from rest_framework.response import Response
    from rest_framework import status
    from . models import employees
    from . serializers import employeesSerializer

    class emloyeeList(APIView):
        def get(self, request):
            employees1 = employees.objects.all()
            serializer=employeesSerializer(employees1,many=True)
            return Response(serializer.data)

        def post(self):
            pass