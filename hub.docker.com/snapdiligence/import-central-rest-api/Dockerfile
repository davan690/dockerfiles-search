FROM microsoft/dotnet:2.1-sdk AS build-env

# Create directory for the app source code
#RUN mkdir -p /usr/src/app

#Change to the app directory
WORKDIR /app

#Set the port number
ENV ASPNETCORE_URLS http://+:80

# Copy the project source file
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM microsoft/dotnet:2.1-aspnetcore-runtime
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "ImportCentralAPI.dll"]