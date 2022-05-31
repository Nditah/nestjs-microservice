# Nestjs Mircoservice

1. Create 3 New Project with nestjs-cli
_simple-backend_ will be the API gateway, _simple-communication_  not listen to extern application while _simple-analytics_ will listen to both application.


    `nest new sample-backend`

    `nest new sample-communication`

     `nest new sample-analytics`

2. Add the 3 projects to a Vs Workspace and save it

3. Open each of the ms and and install @nestjs/microservices

    `yarn add @nestjs/microservices`

4. Open the `main.ts` file in the communication ms and use the microservice create method.
   We will use the default TCP Connection type for miroservice

```ts
async function bootstrap() {
  const app = await NestFactory.createMicroservice<MicroserviceOptions>(
    AppModule,
    { transport: Transport.TCP },
  );
  await app.listen();
}
bootstrap();
```

5. The `backend-microservice` act as the entry point to the backend. Open it an install  @nestjs/microservices

6. Register the ClientModule imported from the @nestjs/microservices into the App.Module.
   ClientModule allow us to inject other nest microservice applications into the api gateway

```ts
@Module({
  imports: [
    ClientsModule.register([
      { name: 'COMMUNICATION', transport: Transport.TCP },
    ]),
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

Run the application with `yarn start:dev`
