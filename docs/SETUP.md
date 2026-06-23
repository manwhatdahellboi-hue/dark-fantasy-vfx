# Setup Guide - Dark Fantasy VFX

## Prerequisites

Before starting, ensure you have:

1. **Unreal Engine 5.4+**
   - Download from [Epic Games Launcher](https://www.unrealengine.com/)
   - Install to default location

2. **Visual Studio 2022**
   - Desktop development with C++
   - Windows SDK 10.0.22000+

3. **NVIDIA Drivers**
   - Latest Game Ready drivers
   - Check in NVIDIA Control Panel

4. **Git**
   - [Download Git](https://git-scm.com/)

5. **Hardware**
   - RTX 3060 or better (RTX 4070+ recommended)
   - 32GB RAM
   - 200GB available SSD space

## Step-by-Step Installation

### 1. Clone the Repository

```bash
git clone https://github.com/manwhatdahellboi-hue/dark-fantasy-vfx.git
cd dark-fantasy-vfx
```

### 2. Generate Project Files

```bash
# Windows
./GenerateProjectFiles.bat

# Or manually:
# Right-click DarkFantasyVFX.uproject
# Select "Generate Visual Studio project files"
```

### 3. Open in Visual Studio

```bash
# Open solution
start DarkFantasyVFX.sln
```

### 4. Build the Project

- Select **Development Editor** configuration
- Select **Win64** platform
- Click **Build** (or press F7)
- Wait for compilation (5-15 minutes depending on hardware)

### 5. Open in UE5

```bash
# Double-click the project file
double-click DarkFantasyVFX.uproject
```

Or:
- Right-click `DarkFantasyVFX.uproject`
- Select "Open with Unreal Engine 5.4"

## Configuration

### NVIDIA Settings

1. Open **Edit → Project Settings** in UE5
2. Search for "DLSS"
3. Enable DLSS 3
4. Set quality presets (Quality, Balanced, Performance)

### Ray Tracing

1. **Edit → Project Settings**
2. Search for "Ray Tracing"
3. Enable **Ray Traced Global Illumination**
4. Enable **Ray Traced Reflections**
5. Adjust quality settings based on GPU

### Performance

1. **Edit → Editor Preferences**
2. Search for "Real Time Rendering"
3. Enable **Real-time Rendering**
4. Adjust viewport FPS cap (60+ recommended)

## First Run Checklist

- [ ] Project opens without errors
- [ ] Scene loads and renders
- [ ] DLSS appears in graphics settings
- [ ] Ray tracing enabled in viewport
- [ ] Can move camera around level
- [ ] Frame rate stable (60+ FPS)

## Troubleshooting

### Issue: Project won't open

**Solution:**
```bash
# Delete intermediate files
rmdir /s /q Intermediate
rmdir /s /q Binaries
rmdir /s /q Saved

# Regenerate files
./GenerateProjectFiles.bat
```

### Issue: DLSS not showing

**Solution:**
1. Check NVIDIA drivers are updated
2. Verify GPU supports DLSS (RTX 3060+)
3. Delete Plugins/DLSS and re-enable in plugins panel
4. Restart UE5

### Issue: Low FPS/Performance

**Solution:**
1. Check GPU is being used (Not CPU)
2. Reduce ray tracing quality
3. Lower resolution
4. Disable DLSS temporarily
5. Close other applications

### Issue: Visual Studio won't build

**Solution:**
1. Clean solution (Build → Clean)
2. Delete Intermediate folder
3. Regenerate project files
4. Update Visual Studio
5. Check Windows SDK version

## Next Steps

- Read [VFX System Guide](VFX_SYSTEM.md)
- Check [DLSS Integration](DLSS_INTEGRATION.md)
- Review [Performance Tips](PERFORMANCE.md)

## Support

For issues:
1. Check GitHub Issues
2. Review UE5 documentation
3. Check NVIDIA driver updates
4. Post on Unreal Forums
