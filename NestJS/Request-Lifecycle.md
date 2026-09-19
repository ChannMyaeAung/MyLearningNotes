# Request Lifecycle (Cross-Cutting Concerns)

![](/home/cma/Desktop/MyLearningNotes/NestJS/Img/Request_Lifecycle.drawio.png)

**Middleware:** Standard functions that execute before route handlers. Perfect for tasks like logging or parsing raw request bodies. 

- ```bash
  $ nest g middleware middleware/api-key --flat
  
  # or 
  
  $ npm exec nest -- g middleware middleware/api-key --flat
  ```

- 

**Guards:** Determine whether a request is authorized to proceed (e.g., checking standard JWT tokens or role-based access control). 

**Interceptors (Pre-handler):** Bind extra logic before a method executes, allowing you to transform or mutate the incoming request. 

- ```bash
  $ nest g interceptor utils/transform --flat
  ```

- ```typescript
  # transform.interceptor.ts
  import {
    CallHandler,
    ExecutionContext,
    Injectable,
    NestInterceptor,
  } from '@nestjs/common';
  import { Response } from 'express';
  import { map, Observable } from 'rxjs';
  
  @Injectable()
  export class TransformInterceptor<T> implements NestInterceptor {
    intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
      const response = context.switchToHttp().getResponse<Response>();
      const statusCode = response.statusCode ?? 200;
  
      return next.handle().pipe(
        map((data: T) => ({
          statusCode,
          message: 'Success',
          data,
        })),
      );
    }
  }
  ```

- ```typescript
  # main.ts
  import { TransformInterceptor } from './utils/transform.interceptor.js';
  
  async function bootstrap(){
      ...
      app.useGlobalInterceptors(new TransformInterceptor());
      await app.listen(process.env.PORT ?? 3000);
  }
  
  await bootstrap();
  ```

**Pipes:** Used for **Data Transformation** (converting strings to integers) and **Data Validation** (rejecting payloads that don't match a designated Data Transfer Object / DTO). 

- ```bash
  $ npm i class-validator class-transformer
  ```

- ```typescript
  # user.controller.ts
  import {
    Controller,
    Get,
    Query,
    Param,
    Post,
    Body,
    Put,
    ParseIntPipe,
  } from '@nestjs/common';
  import { UserService } from './user.service.js';
  @Controller('user')
  export class UserController{
      constructor(private readonly userService: UserService){}
      
      @GET(':id')
  getUserById(@Param('id', ParseIntPipe) id: number): unknown{
      return this.userService.findOneUser(id);
  }
  }
  
  ```

- ```typescript
  # main.ts
  import { TransformInterceptor } from './utils/transform.interceptor.js';
  import { ValidationPipe } from '@nestjs/common';
  
  async function bootstrap(){
      ...
      
      app.useGlobalPipes(new ValidationPipe());
      app.useGlobalInterceptors(new TransformInterceptor());
      await app.listen(process.env.PORT ?? 3000);
  }
  
  await bootstrap();
  ```

**Interceptors (Post-handler):** Mutate or format the result returned by the controller right before it is sent back to the user. 

**Exception Filters:** Catch unhandled errors across the application layers and automatically format them into a clean, uniform HTTP error response. 