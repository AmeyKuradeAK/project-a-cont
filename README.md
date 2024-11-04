This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

# Getting Started
Use Docker Image

## Installation

### Windows Default
```shell
docker pull ghcr.io/ameykuradeak/project-a-cont:latest
```
### linux/amd64

```shell
docker pull ghcr.io/ameykuradeak project-a-cont:latest@sha256:71e3db38968a3c76de4a7bf8ae8e261ad94b8da5ade9701899b828f9c87bdcc2
```

### Arch
```shell
docker pull ghcr.io/ameykuradeak/project-a-cont:latest@sha256:6803235c3ae506ce9c34cae4d7c0c31329c78167a92a563eaaa2eee2dbe6e386
```
## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!

# Local Development and Modification
Here’s a step-by-step on the first approach, assuming you want to extract the code from the image itself:

## Extracting the Code
Identify the Docker image ID by running:

```bash
docker images
```
Look for the image your friend sent, and note the IMAGE ID.

## Create a container from the image and copy the files out:

```bash
docker create --name temp-container <IMAGE_ID>
docker cp temp-container:/app /path/to/your/local/folder
docker rm temp-container
```
Replace /app with the path where the Next.js app is stored in the image (if you know it), and /path/to/your/local/folder with the location on your local machine where you want to save the files.

## Making Edits to the Code
Once you have the code locally:

1. Navigate to the directory and install any dependencies:
```bash
Copy code
cd /path/to/your/local/folder
npm install
```
2. You can now edit the Next.js files as you like.

## Rebuilding the Docker Image
After making modifications, you can create a new Docker image:

1. Create a Dockerfile if one isn’t provided. It might look like this:
Dockerfile

```shell
# Start with a Node.js base image
FROM node:18-alpine

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json, then install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the app code
COPY . .

# Build the Next.js app
RUN npm run build

# Expose the port that Next.js runs on
EXPOSE 3000

# Run the Next.js app
CMD ["npm", "start"]
```
2. Build the Docker image:
```bash

docker build -t my-nextjs-app .
```
3. Run the new Docker image:
```bash
docker run -p 3000:3000 my-nextjs-app
```
This process will give you control over the app’s code and allow you to test modifications in a new Docker image.