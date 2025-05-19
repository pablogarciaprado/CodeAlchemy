◀️ [Home](../../../README.md)

## Basic Concepts
### Comparisons
#### **Cloud Run** vs **Google Kubernetes Engine (GKE)**
Both Cloud Run and Google Kubernetes Engine (GKE) are serverless platforms offered by Google Cloud, but they cater to different needs and offer distinct advantages. Here's a comparison:

| Feature | Cloud Run | Google Kubernetes Engine (GKE) |
|---------|-----------|-------------------------------|
| Abstraction Level | Fully managed, higher abstraction. You focus on code, not infrastructure. | More control and customization. You manage Kubernetes clusters and deployments. |
| Scalability | Scales automatically from zero to handle traffic spikes. Ideal for sporadic or unpredictable workloads. | Highly scalable, but requires manual configuration or autoscaling setup. |
| Cost | Pay only for resources consumed during request execution. Can be very cost-effective for infrequent workloads. | Pay for cluster resources even when idle. More cost-effective for consistently high-traffic applications. |
| Deployment | Deploy containerized applications directly from source code or container images. | Deploy containerized applications using Kubernetes deployments and YAML configurations. |
| Learning Curve | Easier to learn and use, especially for developers new to containerization. | Steeper learning curve due to Kubernetes concepts and management. |
| Control & Flexibility | Less control over underlying infrastructure. Limited customization options. | More control over infrastructure, networking, and security. Highly customizable. |
| Use Cases | Microservices, APIs, event-driven applications, web apps with variable traffic. | Complex applications, machine learning workloads, stateful applications, large deployments. |

In a nutshell:
- Choose Cloud Run for: Simplicity, rapid deployment, automatic scaling, cost-effectiveness for sporadic workloads.
- Choose GKE for: Maximum control, customization, complex applications, high-traffic and stateful workloads.

> Ultimately, the best choice depends on your specific application requirements, technical expertise, and desired level of control.

### Definitions
#### Serverless Computing
Serverless computing on Google Cloud lets you develop and deploy highly scalable applications on a fully managed serverless platform. Services are automatically scaled up and down depending on traffic.

### Files
#### `Dockerfile`
`Dockerfile` specifies the starting image (node:16-slim) and contains the list of commands that are run to build the container image that will host our service. The installation includes installing Imagemagick, which will be used to create thumbnail images from uploaded images.
```
# Copyright 2022 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM node:18-slim

RUN set -ex; \
  apt-get -y update; \
  apt-get -y install imagemagick; \
  rm -rf /var/lib/apt/lists/*; \
  mkdir /tmp/source; \
  mkdir /tmp/destination;

WORKDIR /orch-choreo/services/create-thumbnail
COPY package*.json ./
RUN npm install --production
COPY . .
CMD [ "npm", "start" ]
```

#### `package.json`
`package.json` holds metadata relevant to building your Node.js application. It defines the command that starts the application (node index.js) and specifies the versions of packages used by the code.
```
{
	"name": "create_thumbnail",
	"version": "0.0.1",
	"main": "index.js",
	"scripts": {
	  "start": "node index.js"
	},
	"dependencies": {
	  "@google-cloud/firestore": "^6.0.0",
	  "@google-cloud/storage": "^6.3.0",
	  "bluebird": "^3.7.2",
	  "body-parser": "^1.20.0",
	  "express": "^4.18.1",
	  "imagemagick": "^0.1.3"
	}
}
```