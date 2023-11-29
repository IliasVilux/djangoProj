# Mi Proyecto con Django y Docker

¡Hola! 👋 Este es mi primer proyecto utilizando Django y Docker.
Estoy emocionado/a de aprender y explorar estas tecnologías. A lo
largo de este repositorio iré comentando como voy dando mis primeros pasos.

Python🐍 es un lenguaje que va más allá de la programación. Es una herramienta versátil que afronta desafíos con elegancia y legibilidad. Mi motivación para aprender Python se encuentra en su simplicidad y potencia, y su omnipresencia en áreas como desarrollo web, ciencia de datos y automatización. En este viaje, me baso en la [documentación oficial de Python](https://docs.python.org/3/) para una base sólida y en la lista [Curso de PYTHON Desde Cero](https://www.youtube.com/playlist?list=PLNdFk2_brsRdgQXLIlKBXQDeRf3qvXVU_) de Moure Dev 🚀. ¡Emocionado/a por descubrir las infinitas posibilidades que Python ofrece! ✨

Y en cuanto a Django y Docker, estoy siguiendo la documentación y tutoriales de:

- [HolaMundo - Aprendiendo Docker](https://www.youtube.com/watch?v=4Dko5W96WHg&t=3188s)
- [Fazt - Curso de Django](https://www.youtube.com/watch?v=T1intZyhXDU)
- [Fernando Guerrero - Cómo Dockerizar un Proyecto de Django](https://www.youtube.com/watch?v=envBGHR3eBg)

# 👣 Mis primeros pasos

🛠️ Para empezar con este primer proyecto empecé creando una maquina virtual con Oracle VirtualBox con la imagen de Ubuntu 22.04.3 (Jammy).
Una vez instaladas todas las herramientas en esta máquina (VS Code, Python, Django, Docker, entre otras), empecé creando un entorno virtual:
```
virtualenv nombreDeEntornoVirtual
```
Después de tenerlo creado, lo he tenido que activar:
```
source bin/activate
```
🚀 Ahora ya lo tenía listo para crear mi proyecto en Django.
```
django-admin startproject config .
```
Con el siguiente comando comprové que mi proyecto ya estaba funcionando!:
```
python manage.py runserver
```
Para poder tener nuestro proyecto en un contenedor de docker 🐳 creamos y configuramos el archivo Dockerfile:
```
FROM python:3.12.0-alpine3.18

WORKDIR /app

RUN apk update \
    && apk add --no-cache gcc musl-dev postgresql-dev python3-dev libffi-dev \
    && pip install --upgrade pip

COPY ./requirements.txt ./

RUN pip install -r requirements.txt

COPY ./ ./

CMD [ "python", "manage.py", "runserver", "0.0.0.0:8000" ]
```

El archivo requirements.txt debemos crearlo, de lo contrario no podremos ejecutar el build:
```
pip freeze > requirements.txt
```
Ahora ya podremos hacer el construir y desplegar nuestro contenedor 🚢:
```
docker build -t djangoproj/docker-django .
docker run -p8000:8000 djangoproj/docker-django
```