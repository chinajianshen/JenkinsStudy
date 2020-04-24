#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#net core运行时 引入镜像
#FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base

#设定工作目录
WORKDIR /app

#在容器中程序使用端口，一定要和程序启动使用的端口对应上
EXPOSE 80

#SDK用来还原程序，编译应用
#FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY ["JenkinsStudy/JenkinsStudy.csproj", "JenkinsStudy/"]
RUN dotnet restore "JenkinsStudy/JenkinsStudy.csproj"

#复制文件到工作目录
COPY . .


WORKDIR "/src/JenkinsStudy"
RUN dotnet build "JenkinsStudy.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "JenkinsStudy.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

#启动程序
ENTRYPOINT ["dotnet", "JenkinsStudy.dll"]