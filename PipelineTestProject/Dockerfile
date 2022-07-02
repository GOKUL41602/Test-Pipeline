#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["PipelineTestProject/PipelineTestProject.csproj", "PipelineTestProject/"]
RUN dotnet restore "PipelineTestProject/PipelineTestProject.csproj"
COPY . .
WORKDIR "/src/PipelineTestProject"
RUN dotnet build "PipelineTestProject.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PipelineTestProject.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PipelineTestProject.dll"]