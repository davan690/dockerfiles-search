FROM jetbrains/teamcity-agent:2019.1.3-linux

RUN sh -c "apt-get update && \  
apt-get install -y wget && \
rm -rf /var/lib/apt/lists/* && \
wget https://download.visualstudio.microsoft.com/download/pr/24de5026-3471-4803-b674-689b338569df/10770879c266d430f4707e4f05f14559/dotnet-host-3.0.0-preview8-28405-07-x64.deb && \
wget https://download.visualstudio.microsoft.com/download/pr/f65168d8-ecae-473a-a5ca-4300ed6d82ca/1ef8f137d05783380d128550fb1f0eb8/dotnet-hostfxr-3.0.0-preview8-28405-07-x64.deb && \
wget https://download.visualstudio.microsoft.com/download/pr/b19fcec7-447a-465a-b1d5-b18084485b8d/40017d81e52d89f3aaac2537357c6c63/dotnet-runtime-3.0.0-preview8-28405-07-x64.deb && \
wget https://download.visualstudio.microsoft.com/download/pr/13e08a26-96aa-4b54-ab4f-ab967fa13be2/1bbd4e9a5f04e254ec43ede7f43e7005/aspnetcore-runtime-3.0.0-preview8.19405.7-x64.deb && \
wget https://download.visualstudio.microsoft.com/download/pr/cb26477c-ca65-4fa7-86cc-de0131303875/b844b61ec6ddab5d82dabc6c5efd419b/dotnet-targeting-pack-3.0.0-preview8-28405-07-x64.deb && \
wget https://download.visualstudio.microsoft.com/download/pr/0afc82ac-e13e-4079-8579-0b17ead6479a/37153d3072e9f32ae816664b40302c28/dotnet-apphost-pack-3.0.0-preview8-28405-07-x64.deb && \
wget https://download.visualstudio.microsoft.com/download/pr/90cf071f-8412-45b1-9237-ade8d9d5b871/adcdf7d67cb0554f6a53af3d3b303e9d/aspnetcore-targeting-pack-3.0.0-preview8.19405.7.deb && \
wget https://download.visualstudio.microsoft.com/download/pr/4caf9463-865a-4265-83e6-15fc4c217eb2/320dde5e522457a91b2a1daaa57bb764/netstandard-targeting-pack-2.1.0-preview8-28405-07-x64.deb && \
wget https://download.visualstudio.microsoft.com/download/pr/3dd3123a-7ada-40d0-b920-cd9b22cdb172/5e9142450ceec2b15829ca0fe5c54a68/dotnet-sdk-3.0.100-preview8-013656-x64.deb && \
wget https://download.visualstudio.microsoft.com/download/pr/54a51271-ab58-41f3-9b18-a939888251fe/9b44470e8bc2bb6259e91696ad4e56b5/dotnet-runtime-deps-3.0.0-preview8-28405-07-x64.deb && \
sudo dpkg -i dotnet-runtime-deps-3.0.0-preview8-28405-07-x64.deb && \
sudo dpkg -i dotnet-host-3.0.0-preview8-28405-07-x64.deb && \
sudo dpkg -i dotnet-hostfxr-3.0.0-preview8-28405-07-x64.deb && \
sudo dpkg -i dotnet-runtime-3.0.0-preview8-28405-07-x64.deb && \
sudo dpkg -i aspnetcore-runtime-3.0.0-preview8.19405.7-x64.deb && \
sudo dpkg -i dotnet-targeting-pack-3.0.0-preview8-28405-07-x64.deb && \
sudo dpkg -i dotnet-apphost-pack-3.0.0-preview8-28405-07-x64.deb && \
sudo dpkg -i aspnetcore-targeting-pack-3.0.0-preview8.19405.7.deb && \
sudo dpkg -i netstandard-targeting-pack-2.1.0-preview8-28405-07-x64.deb && \
sudo dpkg -i dotnet-sdk-3.0.100-preview8-013656-x64.deb"
