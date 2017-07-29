FROM microsoft/aspnetcore:1.1
 
WORKDIR /app
COPY bin/Debug/netcoreapp1.1/publish .
 
ENV ASPNETCORE_URLS http://+:5000
EXPOSE 5000
 
ENTRYPOINT ["dotnet", "app.dll"]