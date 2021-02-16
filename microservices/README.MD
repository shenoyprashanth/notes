# Microservices

###  Why should you adopt a Microservice architecture?

-  Provides developer autonomy in terms of choice of technology.

- Allows an organization to quickly experiment things.

- Independent deployability which generally takes away the pain of massive releases.

- Better cohesion . *Code that changes together can stay together*

### What do you lose when you adopt a Microservice architecture?

- You need a bunch of tooling around deployment, observability etc. Complexity!

- The architecture should be prepared to handle network failures.

- Additional latency.

### Does modular monolith work?

- Yes. Proven architecture with plugin based mechanism. However, needs a lot of discipline.

### Can a Microservice contains only APIs?

- Not necessarily. The UI and the API powering the UI can be both part of the same Microservice. 

- Tech stack becomes a bit hairy .

- Each widget in the UI can also have a Microservice serving it. This way if one widget fails, the rest can still work.

- Microfrontend - Break front end into smaller chunks. [Read more about this]

### Service Mesh

-  When a Microservice M1 is invoking Microservice M2  stuff such as service discovery, loadbalancing , fail over etc should be handled.

- Instead of using a library to achieve this, another service (called service mesh, side car) can handle it. 

- M1 calls side car, which then does discovery, loadbalancing , retries etc.

### How are Microservices released?

- **Canary release** - Run the new version of Microservice for only a subset of the users.

- **Blue green deployment** - Deploy the new version of Microservice side by side with the old one. If everything works as expected, stop traffic to the old instances.

### So I have a monolithic application, how do I migrate it to Microservice architecture?

- Pull out a small piece of code, wrap it within a service. Redirect calls from the monolith to new service. **Strangler pattern**

### Ownership of services

- Orphaned services. How do we deal with this? The original developer has either left or has other things to tend to.
