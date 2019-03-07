#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:3.0-aspnetcore-runtime-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:3.0-sdk-nanoserver-1809 AS build
WORKDIR /src
COPY ["WebAppMVC/WebAppMVC.csproj", "WebAppMVC/"]
RUN dotnet restore "WebAppMVC/WebAppMVC.csproj"
COPY . .
WORKDIR "/src/WebAppMVC"
RUN dotnet build "WebAppMVC.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "WebAppMVC.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "WebAppMVC.dll"]