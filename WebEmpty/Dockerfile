#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:3.0-aspnetcore-runtime-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:3.0-sdk-nanoserver-1809 AS build
WORKDIR /src
COPY ["WebEmpty/WebEmpty.csproj", "WebEmpty/"]
RUN dotnet restore "WebEmpty/WebEmpty.csproj"
COPY . .
WORKDIR "/src/WebEmpty"
RUN dotnet build "WebEmpty.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "WebEmpty.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "WebEmpty.dll"]