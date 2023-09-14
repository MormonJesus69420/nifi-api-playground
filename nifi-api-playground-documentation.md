# TOP

This is a collection of useful API endpoints for accessing data that is hard to get in regular NiFi GUI.

Most useful endpoints are the Provenance endpoints, as looking through Provenance table in GUI can be a hassle. 
API gives a nicely searchable JSON structure for extracting information from several Events at a time.
With the help of endpoints you can access hundreds of events at the same time, extract data you need.
Then you can use that data to make more calls to get even more information about what happened in your NiFi.

## Useful Links

Here are links to some other useful resources to get you going:

- [NiFi Homepage](https://nifi.apache.org)
- [NiFi REST API Documentation](https://nifi.apache.org/docs/nifi-docs/rest-api)
- [Description of types of Provenance Events in NiFi](https://nifi.apache.org/docs/nifi-docs/html/user-guide.html#provenance_events)


## Getting Started

### For a Locally Running NiFi Instance

In order to call the NiFi API locally you will need to make sure that you have a NiFi instance set up and running on your local machine.  
Make sure that you have the following information in order:

- **Port Number**, should be 9443 unless you have changed something
- **Username and Password**, get it from logs if you haven't configured a desired username and password yourself

Use these values to set the `base-url`, `username`, and `password` variables in the Postman Environment.

### For a NiFi Instance on a Server

If you wish to call NiFi API on a instance running on a server you will need to have following info:

- **Hostname and Port Number**, as in the case of local instance you need to know the port number, as well as the hostname
- **Your AD Username and Password**, in the case of our NiFi server we use AD usernames and passwords for authentication and authorization 

Use these values to set the `base-url`, `username`, and `password` variables in the Postman Environment.

### Explanation of Environment Variables

This collection uses following Environment Variables:

- `base-url`: the base URL for accessing API endpoints
  - **Example value**: "https://localhost:9443/nifi-api"
- `username`: the username of the user that will access the API endpoints
  - **Example value**: "admin"
- `password`: the password of the user that will access the API endpoints
  - **Example value**: "password123"
- `access-token`: the JWT token for authorization, you get it from the login endpoint
  - **Example value**: "eyJraWQ..."
- `connection-id`: the UUID based identifier of a Connection you want to get more information about
  - **Example value**: "6a3a8f39-018a-1000-5f52-795073c622c4"
- `listing-request-id`: the UUID based identifier of a Listing Request you get when asking for more information about FlowFiles in a Connection
  - **Example value**: "8d6ce287-018a-1000-480d-2f766b5705b0"
- `flowfile-id`: the UUID based identifier of a FlowFile you want to get more information about
  - **Example value**: "03150786-f970-40ee-9e40-e33e19c3c701"
- `provenance-request-id`: the UUID based identifier of a Provenance Request you get when asking for Provenance
  - **Example value**: "8d6e2b90-018a-1000-866c-dce661f59e2f"
- `provenance-event-id`:  the numeric identifier of a Provenance Event in a Provenace Request
  - **Example value**: "13235"

## Authentication/Authorization

In order to authenticate yourself make sure that you have done what is required in the "Getting Started".
Go to the "log-in-to-get-acces-token" endpoint and call it.

You should get a JWT token as a response.  
Save it in the Postmant Environment as `access-token`.  
Then open the cookies overview (link right under the SEND button), and clear all cookies.

You should delete the cookies as they have caused me issues during testing resulting in getting unauthorized error sometimes.

## Structure

As you can see there are three folders with different endpoints.
They are divided mostly by their purpose, authorization, getting info about Connections, and getting info about Provenance Events.
Clicking on the folder should give you some more information about the endpoints located there.
Each endpoint contains documenation, as well as examples.



# AUTH

This folder contains endpoints relating to accessing the NiFi instance. Their purpose is quite self explanatory.

> As noted previously, you need to use the JWT token to authenticate yourself, and _**delete all cookies**_ that get assigned at log in as they will likely cause you issues.



# CONNECTIONS

This folder contains endpoints relating to getting information about Connections between Processors, Ports, and Funnels.

You can get general information about Connections or use Listing Requests to get more in-depth information about the Connection itself and FlowFiles in its queue.

> In order to get the Connection ID, you will have to right click on the connection in NiFi interface, go to _"**Configure**"_ and look for `Id` in the _"**Settings**"_ page.

While it's not strictly necessary, please remember to delete the listing request when you are done with it.



# PROVENANCE

This folder contains endpoints relating to getting information about Provenance events and FlowFiles involved in these events.

It allows you to see how you can filter the Provenance Requests, send Provenance Request, see Provenance Events and investigate FlowFile contents before and after the event.

> In this case you _**have to delete the Provenance Requests**_ when you are done with them, not deleting requests you send will lead to NiFi instance denying receiving more requests.