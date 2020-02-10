# Lab 1 - Using Docker Images

This part of the workshop is an introduction to Docker, which is a runtime for containers. You will create a containerized Node.js application that provides a service to translate phrases from one language to another. The application uses IBM Watson in the IBM Language Translation service.‌ In doing so, you will get to build, ship, and run a container image to understand how Docker works.

## Docker

For this Lab you will use Docker, a container runtime. As seen in this image, Docker provides an interface for containers to interact with the underlying operating system (OS).

![The Docker architecture: Containers are separated from the OS](../.gitbook/assets/assets_-LtBxDkdPh1ZKmLAzW5v_-Ltht0_vGCm5brrUQOK2_-LthwLP_WM_81c65yfLd_image.png)

{% hint style="info" %}
Docker's container technology separates applications from the underlying OS and infrastructure, which is somewhat analogous to virtual machine technology that separates an OS from the baremetal server hardware.
{% endhint %}

Docker technology virtualizes the OS, and only the application and its specific dependencies like libraries and binaries are packaged in the image.

Starting such an image is much faster since it omits the start of an OS. In addition, the image is more portable among various host servers running the Docker engine. And since there is no OS, there isn't as large of a surface for security vulnerabilities connected to it.

## Lab

To get started, you will create a Node.JS-based translation microservice using IBM Watson to respond to requests with translation results. As soon as you set up the microservice, you will build an image, push it to a public repository, and run the containerized microservice.

![This Docker-based part of this workshop is about building, shipping, and running containers.](../.gitbook/assets/assets_-LtBxDkdPh1ZKmLAzW5v_-Ltht0_vGCm5brrUQOK2_-Lthvuq8uvz3mrYS5g_n_image.png)

### Step 1 - Create a language translation service

‌Open your IBM Cloud dashboard using your IBM Cloud account at [https://cloud.ibm.com](https://cloud.ibm.com). Open the **Catalog** tab.

![Select the **Catalog** tab.](../.gitbook/assets/assets_-LtBxDkdPh1ZKmLAzW5v_-LthwpghMpbyDOP2ihex_-LthxWLaGzjKdFOIbJSH_image.png)

Search for `translator` to find the service. You can also find the service by navigating to the **AI** section on the left bar.

![Search for the "Language Translator" service](../.gitbook/assets/assets_-LtBxDkdPh1ZKmLAzW5v_-LthxikOFuFRB2luUf2R_-LthyE7dkUqWr8YzuWKh_image.png)

Select the service to create a new instance. Pick the Lite **free of charge** plan on the next page, and select **Create** to finish creating the service.

![Pick free of charge Lite plan, and select the **Create** button](../.gitbook/assets/assets_-LtBxDkdPh1ZKmLAzW5v_-LthxikOFuFRB2luUf2R_-LthyWbVChchkF_BAAAO_image.png)

You will be redirected to the service landing page.

### Step 2 - Copy the credentials to be used later

Select **Service Credentials* on the left bar. Copy and store the API key for later.

![Copy the API key for future reference](../.gitbook/assets/assets_-LtBxDkdPh1ZKmLAzW5v_-LthxikOFuFRB2luUf2R_-Lthypg9fN1FbyPE_G4X_image.png)

If you do not see a credential provided for you, you can create a new set of credentials. Save your API key somewhere for the next section in this workshop.

**Congratulations!** You created your first language translator service. The next steps will show you how to build a Docker container for a Node.JS application that provides an endpoint to translate text.

### Step 3 - Clone a demo repository

Open your local terminal or the web terminal provided in the workshop and change to the `/data` directory. Clone the repo from [https://github.com/lidderupk/nodejs-docker.git](https://github.com/lidderupk/nodejs-docker.git) to that directory. 

```bash
mkdir data
cd data
git clone https://github.com/lidderupk/nodejs-docker.git
```

### Step 4 - Build a docker image

Change into the directory you just cloned and build the Docker image.

```
cd nodejs-docker
docker build -t <docker-username>/node-container .
```

The `docker-username` is required if you want to publish your image to Docker Hub. Replace `<docker-username>` in the above command with your Docker account name.

{% hint style="info" %}
If you do not have a Docker account, you will need to create one directly on hub.docker.com by selecting the **Sign Up** button.
{% endhint %}

Alternatively, you can also build directly from GitHub using the following command without cloning the repository:

```bash
docker build -t <docker-username>/node-container https://github.com/lidderupk/nodejs-docker.git
```

This command uses the Dockerfile to download a Node.JS 10 base image and then install our Express.JS application on top. Let's explore the contents of this Dockerfile:

```
FROM node:10
```

... builds our image on top of the Node.js 10 image.

```
WORKDIR /usr/src/app
```

... creates a working directory for our application to live in.

```
COPY package*.json ./
```

... copies the package.json file to our working directory.

```
RUN npm install
```

... install our dependencies. We just have two dependencies in this application: express and ibm-watson.

```
COPY . .
```

... copy the rest of our source code into the docker image

```
EXPOSE 8080
```

... expose port 8080. We will still have to forward our local port to this docker container port later.

```
CMD [ "node", "server.js" ]
```

... starts the application by running node server.js.

### Step 5 - Run the Docker image

```bash
docker run -p 8080:8080 -e "nlp_key=<api_key>" -d <docker-username>/node-container
```

In this example case, I would run

```bash
docker run -p 8080:8080 -e "nlp_key=T1ReDZISYE4cpqQnQHKTWe1F9iUy6hhxkRu0aWqzmxQ3" -d upkar/node-container
```

### Step 6 - Test the application

```bash
curl "localhost:8080/translate?text=how%20are%20you"
```

You should see output as follows:

```json
{
  "translations": [
    {
      "translation": "¿Cómo estás?"
    }
  ],
  "word_count": 3,
  "character_count": 11
}%
```

The text is translated to Spanish (en-sp) by default. You can specify the language by passing in the `lang` flag as follows:

```bash
curl "localhost:8080/translate?text=how%20are%20you?&lang=en-de"
```

You should now see the same text translated to German:

```json
{
  "translations": [
    {
      "translation": "Wie geht es Ihnen?"
    }
  ],
  "word_count": 3,
  "character_count": 12
}
```

Another example...

```bash
curl "localhost:8080/translate?text=People+are+suffering.+People+are+dying+and+dying+ecosystems+are+collapsing.+We+are+in+the+beginning+of+a+mass+extinction%2C+and+all+you+can+talk+about+is+the+money+and+fairy+tales+of+eternal+economic+growth"
```

```json
{
  "translations": [
    {
      "translation": "La gente está sufriendo. La gente está muriendo y los ecosistemas moribundos se están derrumbando. Estamos en el principio de una extinción masiva, y todo lo que se puede hablar es el dinero y los cuentos de hadas del crecimiento económico eterno"
    }
  ],
  "word_count": 39,
  "character_count": 204
}
```

in Polish

```bash
curl "localhost:8080/translate?text=People+are+suffering.+People+are+dying+and+dying+ecosystems+are+collapsing.+We+are+in+the+beginning+of+a+mass+extinction%2C+and+all+you+can+talk+about+is+the+money+and+fairy+tales+of+eternal+economic+growth&lang=en-pl"
```
```json
{
  "translations": [
    {
      "translation": "Ludzie cierpią. Ludzie umierają, a umierające ekosystemy się zapadają. Jesteśmy na początku masowego wyginięcia, a wszystko, o czym można rozmawiać, to pieniądze i bajki o wiecznym wzroście gospodarczym"
    }
  ],
  "word_count": 39,
  "character_count": 204
}
```

You can see the supported languages (both from and to) in the language translator documentation.

**Congratulations!** You just containerized a Node.js application that provides translation services.


### Step 7 - Cleaning up

First, stop the container. You need the container tag or the id to stop it. Use `docker ps | grep <image-name>` to get those details.

```bash
docker ps | grep node-container

419104eff9be        upkar/node-container                           "docker-entrypoint.s…"   3 minutes ago       Up 3 minutes                       0.0.0.0:8080->8080/tcp                            cranky_davinci
```

In this example, the container is called `cranky_davinci` and has an id of `419104eff9be`.

Stop the image with `docker stop <container_id`. You can replace the id with your container id.

```bash
docker stop 419104eff9be
```

Run `docker rm <container_id>` to remove the container. Replace the id with your container id identified previously.

```bash
docker rm 419104eff9be
```

### Step 8 - Removing the image

You can now delete the image. First, get the image id using `docker images` and `grep`:

```bash
$ docker images | grep node-container
upkar/node-container                       latest                         8baa6ca9cdac        5 minutes ago       958MB
```

Now, delete the image using `docker image rm <image_id>`":

```bash
$ docker image rm 8baa6ca9cdac
Untagged: upkar/node-container:latest
Deleted: sha256:8baa6ca9cdac8868d8e17642e90b433c7aa588a615b59ac9b528fb8635698a6e
Deleted: sha256:8c279f530b3ff260279f9cb8d22d167d748e53df4f6eab91b089b6c90b4da9f2
Deleted: sha256:9845fb86e05aa69c677dc999b6bbdeceafc460e70604092e0247e0f8880ec93e
Deleted: sha256:84ea0148452752e2aba0a1e1ef963355781a3fb87e485e9a440e8e8fc002d045
Deleted: sha256:f4f99031ae0d1cf0079d9981ef6e8dff2231eff670ac7ea4a4f27472282d7ad2
Deleted: sha256:bf579e37b35bb7b20ed0cb3140220118cbbd1d0564a512f21868568b3683a392
Deleted: sha256:07b958722eb2513e88186b2f6eddadcec0b8772001a598244352073ac5caf176
Deleted: sha256:361c46840912a7b9539b6ddf00164492fc594701085f0e2c9d2c1544bca8498c
```

## Troubleshooting

Check the container logs.

You can check the logs for your container using `docker logs <container_id>`:

For example:

```bash
root@terminal-5-846448d675-bk75j:/data# docker logs 4450279a9f50

Running on http://0.0.0.0:8080
No language passed to translate to. Converting to Spanish by default.
{
  "translations": [
    {
      "translation": "Hola"
    }
  ],
  "word_count": 1,
  "character_count": 5
}
```

**Congratulations** again on creating your first Docker container that hosts a natural language translation service!
