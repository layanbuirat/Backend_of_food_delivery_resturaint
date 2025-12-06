<<<<<<< HEAD
﻿FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["FoodDeliveryAPI.csproj", "."]
RUN dotnet restore "./FoodDeliveryAPI.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "./FoodDeliveryAPI.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./FoodDeliveryAPI.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app

# 1️⃣ إنشاء مجلد Data للملفات الدائمة
RUN mkdir -p /app/Data

# 2️⃣ نسخ قاعدة البيانات إذا كانت موجودة
COPY --from=publish /app/publish/Data/* /app/Data/ || true

# 3️⃣ نسخ باقي الملفات
COPY --from=publish /app/publish .

# 4️⃣ تأكد من صلاحيات الكتابة
RUN chmod -R 755 /app/Data

=======
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["FoodDeliveryAPI.csproj", "."]
RUN dotnet restore "FoodDeliveryAPI.csproj"
COPY . .
RUN dotnet build "FoodDeliveryAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "FoodDeliveryAPI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
>>>>>>> 8f57ef06fd06a93ddac0cac7a2b4f6a1c386ab3d
ENTRYPOINT ["dotnet", "FoodDeliveryAPI.dll"]