FROM mcr.microsoft.com/dotnet/sdk:5.0.100-rc.1-buster-slim AS build
WORKDIR /app

COPY /BlazorScopedStyles ./BlazorScopedStyles
COPY /BlazorShared ./BlazorShared

RUN dotnet restore /app/BlazorScopedStyles/BlazorScopedStyles.csproj

WORKDIR /app/BlazorScopedStyles
RUN dotnet build -c Release -o /bin/Release/net5.0

FROM build AS publish
RUN dotnet publish -c Release -o /bin/Release/net5.0/browser-wasm/publish

FROM nginx:alpine AS final

WORKDIR /usr/share/nginx/html

COPY --from=publish /bin/Release/net5.0/browser-wasm/publish/wwwroot /usr/local/webapp/nginx/html
COPY --from=build /bin/Release/net5.0/nginx.conf /etc/nginx/nginx.conf
EXPOSE 80