# Data Transfer Object (DTO)

- An object that defines how data will be sent over the network.
- NestJS recommends using *Javscript Classes* as Typescript interfaces are completely removed during compilation aka transpilation to Javascript. Classes, however, are part of the ES6 standard and remain preserved as real entites at runtime.
  - Because classes exist at runtime, NestJS features like **Pipes** can access the metatype of the variable to perform automatic data validation and transformation.

---

# Core Responsibilities

1. **Input Validation:** In combination with the `class-validator` library and the built-in `ValidationPipe`, DTO decorators evaluate incoming payloads at runtime to ensure they match required constraints (e.g., verifying an email format or checking string lengths).
2. **Data Transformation:** DTOs work with `class-transformer` to sanitize payloads, automatically stripping out unexpected properties or parsing strings into numbers. 
3. **Response Serialization:** DTOs can filter out sensitive entity data (like hashed passwords) before sending a JSON response back to the client. 
4. **Auto-Generated Documentation:** When using packages like `@nestjs/swagger`, our DTO classes serve as the foundation for generating interactive API documentation automatically.

---

## Code Implementation Example

```typescript
// create-user.dto.ts
import { IsEmail, IsString, MinLength } from 'class-validator';

export class CreateUserDto {
  @IsString()
  name: string;

  @IsEmail()
  email: string;

  @IsString()
  @MinLength(6, { message: 'Password must be at least 6 characters long' })
  password: string;
}
```

We can then apply this DTO within our Controller layer using the `@Body()` decorator:

```typescript
// users.controller.ts
import { Controller, Post, Body, UsePipes, ValidationPipe } from '@nestjs/common';
import { CreateUserDto } from './dto/create-user.dto';

@Controller('users')
export class UsersController {
  
  @Post()
  @UsePipes(new ValidationPipe()) // Activates runtime validation for the DTO
  async create(@Body() createUserDto: CreateUserDto) {
    // createUserDto is guaranteed to have valid data at this point
    return this.usersService.create(createUserDto);
  }
}
```

---

## Difference Between DTOs and Entities

| Feature | DTO                                                          | Entity                                                       |
| ------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Layer   | Presentation / Controller layer                              | Database / Data Access Layer                                 |
| Purpose | Defines the structure of data in transit over the network    | Defines the structure of data at rest inside the database    |
| Fields  | Contains only the fields required for that specific API request/response. | Contains all database columns, keys, and internal relation IDs. |

