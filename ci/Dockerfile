FROM microsoft/dotnet:2.2-sdk AS build-env
COPY . /app
WORKDIR /app

RUN dotnet restore 
RUN dotnet publish -c Release -o out
 
# Build runtime image
FROM microsoft/dotnet:2.2-aspnetcore-runtime
ARG SERVICE_BUS_CONNECTION_STRING
ENV ServiceBusConnectionString=$SERVICE_BUS_CONNECTION_STRING
ARG SERVICE_BUS_QUEUE_NAME
ENV ServiceBusQueueName=$SERVICE_BUS_QUEUE_NAME

ARG SMTP_SERVER
ENV SmtpServer=$SMTP_SERVER
ARG SMTP_PORT
ENV SmtpPort=$SMTP_PORT
ARG SMTP_USERNAME
ENV SmtpUsername=$SMTP_USERNAME
ARG SMTP_PASSWORD
ENV SmtpPassword=$SMTP_PASSWORD

WORKDIR /app
COPY --from=build-env /app/Mailer/out .
ENTRYPOINT ["dotnet", "Mailer.dll"]