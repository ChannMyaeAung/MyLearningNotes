# AWS API Gateway (Authenticated / Protected Routes via Cognito)

1. After logging in to API-Gateway from AWS console, we can try different types of APIs.

2. Let's say we choose REST_API, and then in the console, we have to give name. (E.g.  `re_api-gateway`)

3. After pressing next, we check "Proxy resource" and "CORS", and type "{proxy+}" for Resource name and then click "Create resource".

4. Then click on "ANY" and "Edit integration".

5. Inside we are going to select "HTTP", check "HTTP proxy integration", set HTTP method to "ANY".

6. Then go back to EC2 instance, grab the public IPv4 address and then inside previous console (API Gateway Panel), paste "http://[EC2-IPv4-Addr]/{proxy}" inside "Endpoint URL".

7. Then hit "Save".

8. From there, we are going to create something called "Cognito Authorizers" which allows Cognito to authorize our API calls that we make from the frontend to the backend.

9. To do that, inside the same API Gateway panel in AWS, we go to "Authorizers", hit "Create an authorizer" and give Authorizer name (e.g. `re_api-gateway-cognito-authorizer`).

10. Then select "Cognito" as Authorizer type and then Select the intended Cognito user pool under "Cognito user pool".

11. For "Token source", we type in "Authorization".

    ![Screenshot from 2026-07-29 18-36-03](/home/cma/Pictures/Screenshots/Screenshot from 2026-07-29 18-36-03.png)

12. Then we click "Create authorizer".

13. After that we go back to "Resources" on the left panel, click on "ANY" and Click "Edit" of the Method request settings.

14. Then on the edit page, For Authorization, we need to select our cognito authorizer (e.g. `re_api-gateway-cognito-authorizer`) and then hit "Save".

15. Click **Deploy API**, select the `prod` stage (or create a new stage), and deploy so the Cognito Authorizer changes take effect.



# API Proxy for Public Backend Routes

For public routes, we need to create a new resource,

1. Inside "Resources" on the left panel of AWS API Gateway console, but this time we don't check "Proxy resource" but check "CORS".
2. For Resource name, we give the public endpoint of our application. (e.g. `properties`), and Resource path being /.
3. After that we click "Create method" for the new resource we have created, select "GET" fro Method type, select HTTP for Integration type, check "HTTP proxy integration".
4. For HTTP method, we select GET.
5. For Endpoint URL, we set the same thing followed by our new endpoint name (/properties). `http://[AWS-EC2-Public-IPv4-Addr]/properties`.
6. With all of this we can deploy our API by click "Deploy API". 
7. Upon appearing a Deploy API Modal, for Stage, we select New Stage, and stage name to be "prod".

