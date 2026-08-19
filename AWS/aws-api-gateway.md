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

16. After that if we have deployed our frontend on AWS Amplify, we can copy the "Invoke URL" inside the Stages on the left panel of API Gateway AWS. We can also check our endpoints working or not with that Invoke URL.

17. Now inside the AWS Amplify, go to the hosted application, go to "Hosting", Under Hosting, go to "Environment variables". 

18. And we can update our NEXT_PUBLIC_API_BASE_URL with our newly copied Invoke URL from the AWS API Gateway.

19. After that we can save the updated Environment variables and redeploy our application from the AWS Amplify.



# API Proxy for Public Backend Routes

For public routes, we need to create a new resource,

1. Inside "Resources" on the left panel of AWS API Gateway console, but this time we don't check "Proxy resource" but check "CORS".
2. For Resource name, we give the public endpoint of our application. (e.g. `properties`), and Resource path being /.
3. After that we click "Create method" for the new resource we have created, select "GET" fro Method type, select HTTP for Integration type, check "HTTP proxy integration".
4. For HTTP method, we select GET.
5. For Endpoint URL, we set the same thing followed by our new endpoint name (/properties). `http://[AWS-EC2-Public-IPv4-Addr]/properties`.
6. With all of this we can deploy our API by click "Deploy API". 
7. Upon appearing a Deploy API Modal, for Stage, we select New Stage, and stage name to be "prod".

---

# Things worth Noting

- Upon stopping and restarting any EC2 instance, that EC2 instance will be reassigned a brand new Public IPv4 address.

- If we have any AWS API Gateway running, it will still be configured with the old IP address so it can results in 504 Gateway Timeout on our application.

- Step by Step fix will be the following:

  - Go to the **AWS Console** ➔ **EC2** ➔ **Instances**.

  - Click on the backend instance (`re_ec2`).

  - Copy the new **Public IPv4 address**.

  - Ensure the backend server is running on EC2. We can check this by checking PM2 status via `pm2 status` upon connecting to the EC2 instance via SSH/EC2 instance Connect.

  - If the process is stopped or missing, restart it with:

    ```bash
    pm2 start dist/index.js --name 
    # or: pm2 restart all
    ```

  - Verify it is actively listening on port 80:

    ```bash
    sudo netstat -tlpn | grep 80
    ```

  - Next, Update API Gateway with the New EC2 IP Address.

  - Go to **AWS Console** ➔ **API Gateway** ➔ **`re_api_gateway`**.

  - In the left panel, navigate to **Resources**.

  - Select **`/{proxy+}`** ➔ Click the **`ANY`** method.

  - Select the **Integration request** tab and click Edit.

  - Update the Endpoint URL with our newly copied EC2 IP address like this.

    ```bash
    http://<YOUR-NEW-EC2-PUBLIC-IP>/{proxy}
    ```

  - Click Save.

  - Click the Deploy API button in the top right. Select the `prod` stage and click Deploy.

- ### Permanent Solution: Allocate an Elastic IP

  To prevent the IP address from changing every time you stop/start the EC2 instance:

  1. In the EC2 console left menu, go to **Network & Security** ➔ **Elastic IPs**.
  2. Click **Allocate Elastic IP address** ➔ **Allocate**.
  3. Select the allocated IP ➔ Click **Actions** ➔ **Associate Elastic IP address**.
  4. Choose your EC2 instance and associate it.
  5. Set this static Elastic IP in API Gateway once, and it will persist across all future restarts.
