# Pixel Streaming Guide

## Overview

**Pixel Streaming** allows you to stream your UE5 game from a powerful server to players' browsers, enabling cloud gaming and easy distribution.

## Architecture

```
Player Browser
    ↓ WebRTC
    ↓ Video Stream
Signaling Server
    ↓ Unreal App
    ↓ GPU Rendering
Cloud Server (AWS/Azure/Google Cloud)
```

## Setup

### Prerequisites

1. **Unreal Engine 5.4+** with Pixel Streaming plugin
2. **Cloud hosting** (AWS, Google Cloud, or Azure)
3. **GPU instance** (NVIDIA GPU recommended)
4. **Signaling server** (Node.js)

### Step 1: Enable Pixel Streaming Plugin

1. Open **Edit → Plugins**
2. Search for "Pixel Streaming"
3. Enable **Pixel Streaming** plugin
4. Restart UE5

### Step 2: Package for Streaming

```bash
# Build for shipping (Linux preferred)
UnrealBuildTool -project=DarkFantasyVFX.uproject -debug

# Or use Automation Tool
./Engine/Build/BatchFiles/RunUAT.bat BuildCook \
  -Project=DarkFantasyVFX.uproject \
  -ClientConfig=Shipping \
  -ServerConfig=Shipping \
  -NoEditor \
  -Cook \
  -Package
```

### Step 3: Setup Signaling Server

```bash
# Install Node.js dependencies
cd Pixel Streaming Signaling Server
npm install

# Start signaling server
node SignalingServer.js
```

### Step 4: Deploy to Cloud

#### AWS EC2 Setup

```bash
# Launch GPU instance (g4dn.xlarge or better)
aws ec2 run-instances \
  --image-id ami-0c55b159cbfafe1f0 \
  --instance-type g4dn.xlarge \
  --key-name your-key \
  --security-groups pixel-streaming-sg

# SSH into instance
ssh -i your-key.pem ubuntu@<instance-ip>

# Install dependencies
sudo apt-get update
sudo apt-get install -y nvidia-driver-470
sudo apt-get install -y nodejs npm

# Upload and run Pixel Streaming app
scp -i your-key.pem -r ./DarkFantasyVFX ubuntu@<instance-ip>:/home/ubuntu/

# Start application
cd /home/ubuntu/DarkFantasyVFX
./DarkFantasyVFX/Binaries/Linux/UE4Server \
  -AllowStdOutLogVerbosity \
  -Windowed \
  -ResX=1920 \
  -ResY=1080 \
  -PixelStreamingPort=8888
```

## Performance Optimization

### Streaming Settings

```cpp
// Configure for optimal streaming
void ConfigurePixelStreaming()
{
    // Target bitrate: 10-15 Mbps for 1080p@60fps
    IConsoleVariable* BitrateVar = IConsoleManager::Get().FindConsoleVariable(TEXT("PixelStreaming.Bitrate"));
    if(BitrateVar)
    {
        BitrateVar->Set(12000, EConsoleVariableFlags::ECVF_SetByCommandline);
    }
    
    // FPS cap
    IConsoleVariable* FPSVar = IConsoleManager::Get().FindConsoleVariable(TEXT("PixelStreaming.FrameRate"));
    if(FPSVar)
    {
        FPSVar->Set(60, EConsoleVariableFlags::ECVF_SetByCommandline);
    }
}
```

## Deployment Checklist

- [ ] DLSS enabled for performance
- [ ] Ray tracing optimized
- [ ] Signaling server running
- [ ] WebRTC candidates configured
- [ ] CORS headers set
- [ ] SSL certificates installed
- [ ] Security groups configured
- [ ] Load testing completed
- [ ] Monitor GPU usage
- [ ] Monitor bandwidth

## Troubleshooting

### Connection Issues

1. Check signaling server is running
2. Verify WebRTC port is open
3. Check firewall rules
4. Verify STUN/TURN servers

### Quality Issues

1. Check network bandwidth
2. Reduce resolution/bitrate
3. Check GPU utilization
4. Monitor latency

### Performance Issues

1. Check GPU load
2. Verify CPU is not bottleneck
3. Check memory usage
4. Monitor stream bitrate

## Resources

- [UE5 Pixel Streaming Docs](https://docs.unrealengine.com/5.0/en-US/unreal-engine-pixel-streaming-overview/)
- [AWS GPU Instances](https://aws.amazon.com/ec2/instance-types/g4/)
- [WebRTC Best Practices](https://webrtc.org/getting-started/peer-connections)
