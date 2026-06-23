# Performance Optimization Guide

## Performance Targets

- **Resolution:** 1440p native
- **Frame Rate:** 60+ FPS
- **Ray Tracing:** High quality
- **DLSS:** Enabled (Balanced)
- **GPU Memory:** <8GB VRAM

## Profiling Tools

### Built-in UE5 Profiler

```cpp
// Enable in-game stats
stat unit        // Overall timing
stat gpu         // GPU breakdown
stat particles   // Particle stats
stat physics     // Physics timing
stat streaming   // Memory streaming
```

## Optimization Strategies

### 1. LOD (Level of Detail)

```cpp
// Automatic LOD based on distance
if(Distance > 5000.0f)
{
    SetLOD(4); // Most simplified
}
else if(Distance > 2500.0f)
{
    SetLOD(3);
}
else if(Distance > 1250.0f)
{
    SetLOD(2);
}
else
{
    SetLOD(0); // Full quality
}
```

### 2. Draw Call Optimization

```cpp
// Batch similar objects
for(const AActor* Actor : ActorsInScene)
{
    if(Actor->GetMaterial() == CurrentMaterial)
    {
        // Batch render
        BatchRender(Actor);
    }
}
```

### 3. Texture Optimization

```cpp
// Use compressed textures
Texture->CompressionSettings = TextureCompressionSettings::Default;

// Stream textures based on camera
Texture->bUseStreamingMipLevels = true;
Texture->StreamingDistanceMultiplier = 1.5f;
```

### 4. Particle Optimization

```cpp
// Limit particle count
const int32 MaxParticles = 2000;
const int32 MaxActiveSystems = 50;

// GPU-accelerated particles
Emitter->SetSimulationTarget(EParticleSimulateTarget::GPU);
```

## Memory Management

### Asset Streaming

```cpp
// Stream assets on demand
FStreamableManager StreamManager = GetWorld()->GetStreamableManager();
StreamManager.RequestAsyncLoad(AssetPath, [this](){ /* loaded */ });
```

### Pool System

```cpp
// Object pooling for particles
class FParticlePool
{
public:
    FParticlePool(int32 Size) { Reserve(Size); }
    
    AParticle* GetParticle()
    {
        if(AvailableParticles.Num() > 0)
        {
            return AvailableParticles.Pop();
        }
        return nullptr;
    }
    
    void ReturnParticle(AParticle* Particle)
    {
        Particle->Reset();
        AvailableParticles.Push(Particle);
    }
    
private:
    TArray<AParticle*> AvailableParticles;
};
```

## Ray Tracing Optimization

### Quality Tiers

```cpp
// Ultra (RTX 4090+)
RayTracedGlobalIllumination = true;
RayTracedReflections = true;
RayTracedShadows = true;
NumRayTracingSamples = 64;

// High (RTX 3090)
RayTracedGlobalIllumination = true;
RayTracedReflections = true;
NumRayTracingSamples = 32;

// Medium (RTX 3070)
RayTracedReflections = true;
NumRayTracingSamples = 16;

// Low (RTX 3060)
RayTracedReflections = true;
NumRayTracingSamples = 8;
```

## DLSS Optimization

```cpp
// Optimal DLSS settings
DLSSMode = EDLSSMode::Balanced; // Best balance
DLSSPreset = EDLSSPreset::Quality;

// Enable Frame Generation on RTX 4000+
FrameGeneration = true;
Reflex = true; // Reduce latency
```

## Profiling Checklist

- [ ] Baseline FPS (60+ target)
- [ ] GPU memory usage (<8GB)
- [ ] Draw calls (<1000)
- [ ] Triangle count (<5M visible)
- [ ] Particle count balanced
- [ ] Load times <30 seconds
- [ ] No memory leaks
- [ ] Smooth frame timing

## Common Bottlenecks

### GPU-Bound

**Symptoms:** High GPU utilization (>90%), low CPU

**Solutions:**
1. Reduce resolution
2. Lower ray tracing quality
3. Reduce particle count
4. Simplify shaders

### CPU-Bound

**Symptoms:** High CPU usage (>80%), low GPU

**Solutions:**
1. Reduce draw calls
2. Batch operations
3. Use multithreading
4. Profile hot functions

### Memory-Bound

**Symptoms:** Memory usage >90%, stuttering

**Solutions:**
1. Stream assets
2. Use compression
3. Pool objects
4. Reduce texture resolution

## Performance Targets by GPU

| GPU | Resolution | FPS | Ray Tracing | DLSS |
|-----|-----------|-----|-----------|------|
| RTX 3060 | 1080p | 60+ | Medium | Balanced |
| RTX 3070 | 1440p | 60+ | High | Balanced |
| RTX 3090 | 1440p | 60+ | Ultra | Quality |
| RTX 4070 | 1440p | 60+ | Ultra | Quality |
| RTX 4090 | 4K | 60+ | Ultra | Quality |

## Resources

- [UE5 Performance Guide](https://docs.unrealengine.com/5.0/en-US/performance-and-profiling-in-unreal-engine/)
- [NVIDIA FrameView Profiler](https://developer.nvidia.com/frameview)
- [GPU Performance Analysis](https://developer.nvidia.com/blog/understanding-performance-bottlenecks-in-gpu-rendering/)
