-- Reaper Toll - Combined Hitbox & Noclip System
-- Place this LocalScript in StarterPlayerScripts or StarterGui.

--== SERVICES =================================================================
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer
local gui = LocalPlayer:WaitForChild("PlayerGui")

--== REAPER TOLL SETTINGS ====================================================
_G.HeadSize = 15
_G.HitboxSize = Vector3.new(15, 15, 15)
_G.HitboxDisabled = false
_G.NoclipEnabled = true
_G.HitboxToggleKey = Enum.KeyCode.H
_G.NoclipToggleKey = Enum.KeyCode.N
local detectionRange = 25
local frameUpdateRate = 0.1

--== NOCLIP SYSTEM ===========================================================
local Noclip = nil
local Clip = nil

local function enableNoclip()
Clip = false
local function NoClipLoop()
if Clip == false and LocalPlayer.Character ~= nil then
for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
if v:IsA('BasePart') and v.CanCollide and v.Name ~= "floatName" then
v.CanCollide = false
end
end
end
wait(0.21) -- Basic optimization
end
Noclip = RunService.Stepped:Connect(NoClipLoop)
end

local function disableNoclip()
if Noclip then
Noclip:Disconnect()
Noclip = nil
end
Clip = true
-- Restore collision for all parts
if LocalPlayer.Character then
for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
if v:IsA('BasePart') and v.Name ~= "floatName" then
v.CanCollide = true
end
end
end
end

-- Initialize noclip if enabled
if _G.NoclipEnabled then
enableNoclip()
end

--== REAPER GUI SETUP ========================================================
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ReaperTollGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = gui

-- Main Control Panel
local controlPanel = Instance.new("Frame")
controlPanel.Size = UDim2.new(0, 280, 0, 120)
controlPanel.Position = UDim2.new(0.5, -140, 0.05, 0)
controlPanel.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
controlPanel.BackgroundTransparency = 0.1
controlPanel.BorderSizePixel = 0
controlPanel.Parent = screenGui

-- Control Panel Styling
local controlCorner = Instance.new("UICorner")
controlCorner.CornerRadius = UDim.new(0, 10)
controlCorner.Parent = controlPanel

local controlStroke = Instance.new("UIStroke")
controlStroke.Color = Color3.fromRGB(100, 0, 0)
controlStroke.Thickness = 3
controlStroke.Parent = controlPanel

-- Reaper Title
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -20, 0, 30)
titleLabel.Position = UDim2.new(0, 10, 0, 5)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "💀 REAPER TOLL SYSTEM 💀"
titleLabel.TextColor3 = Color3.fromRGB(200, 0, 0)
titleLabel.TextScaled = true
titleLabel.Font = Enum.Font.SourceSansBold
titleLabel.Parent = controlPanel

-- Hitbox Status Frame
local hitboxFrame = Instance.new("Frame")
hitboxFrame.Size = UDim2.new(0, 250, 0, 35)
hitboxFrame.Position = UDim2.new(0.5, -125, 0, 40)
hitboxFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
hitboxFrame.BackgroundTransparency = 0.2
hitboxFrame.BorderSizePixel = 0
hitboxFrame.Parent = controlPanel

local hitboxCorner = Instance.new("UICorner")
hitboxCorner.CornerRadius = UDim.new(0, 6)
hitboxCorner.Parent = hitboxFrame

local hitboxStroke = Instance.new("UIStroke")
hitboxStroke.Color = Color3.fromRGB(100, 0, 0)
hitboxStroke.Thickness = 2
hitboxStroke.Parent = hitboxFrame

-- Hitbox skull icon
local hitboxSkull = Instance.new("TextLabel")
hitboxSkull.Size = UDim2.new(0, 25, 0, 25)
hitboxSkull.Position = UDim2.new(0, 5, 0.5, -12.5)
hitboxSkull.BackgroundTransparency = 1
hitboxSkull.Text = "💀"
hitboxSkull.TextColor3 = Color3.fromRGB(200, 0, 0)
hitboxSkull.TextScaled = true
hitboxSkull.Font = Enum.Font.SourceSansBold
hitboxSkull.Parent = hitboxFrame

-- Hitbox label
local hitboxLabel = Instance.new("TextLabel")
hitboxLabel.Size = UDim2.new(1, -80, 1, 0)
hitboxLabel.Position = UDim2.new(0, 35, 0, 0)
hitboxLabel.BackgroundTransparency = 1
hitboxLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
hitboxLabel.TextScaled = true
hitboxLabel.Font = Enum.Font.SourceSansBold
hitboxLabel.Text = "The Reaper watches..."
hitboxLabel.Parent = hitboxFrame

-- Hitbox toggle indicator
local hitboxToggle = Instance.new("TextLabel")
hitboxToggle.Size = UDim2.new(0, 35, 0, 25)
hitboxToggle.Position = UDim2.new(1, -40, 0.5, -12.5)
hitboxToggle.BackgroundTransparency = 1
hitboxToggle.Text = "ON"
hitboxToggle.TextColor3 = Color3.fromRGB(0, 255, 0)
hitboxToggle.TextScaled = true
hitboxToggle.Font = Enum.Font.SourceSansBold
hitboxToggle.Parent = hitboxFrame

-- Noclip Status Frame
local noclipFrame = Instance.new("Frame")
noclipFrame.Size = UDim2.new(0, 250, 0, 35)
noclipFrame.Position = UDim2.new(0.5, -125, 0, 80)
noclipFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
noclipFrame.BackgroundTransparency = 0.2
noclipFrame.BorderSizePixel = 0
noclipFrame.Parent = controlPanel

local noclipCorner = Instance.new("UICorner")
noclipCorner.CornerRadius = UDim.new(0, 6)
noclipCorner.Parent = noclipFrame

local noclipStroke = Instance.new("UIStroke")
noclipStroke.Color = Color3.fromRGB(0, 100, 100)
noclipStroke.Thickness = 2
noclipStroke.Parent = noclipFrame

-- Noclip ghost icon
local noclipGhost = Instance.new("TextLabel")
noclipGhost.Size = UDim2.new(0, 25, 0, 25)
noclipGhost.Position = UDim2.new(0, 5, 0.5, -12.5)
noclipGhost.BackgroundTransparency = 1
noclipGhost.Text = "👻"
noclipGhost.TextColor3 = Color3.fromRGB(0, 200, 200)
noclipGhost.TextScaled = true
noclipGhost.Font = Enum.Font.SourceSansBold
noclipGhost.Parent = noclipFrame

-- Noclip label
local noclipLabel = Instance.new("TextLabel")
noclipLabel.Size = UDim2.new(1, -80, 1, 0)
noclipLabel.Position = UDim2.new(0, 35, 0, 0)
noclipLabel.BackgroundTransparency = 1
noclipLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
noclipLabel.TextScaled = true
noclipLabel.Font = Enum.Font.SourceSansBold
noclipLabel.Text = "Noclip: [N]"
noclipLabel.Parent = noclipFrame

-- Noclip toggle indicator
local noclipToggle = Instance.new("TextLabel")
noclipToggle.Size = UDim2.new(0, 35, 0, 25)
noclipToggle.Position = UDim2.new(1, -40, 0.5, -12.5)
noclipToggle.BackgroundTransparency = 1
noclipToggle.Text = _G.NoclipEnabled and "ON" or "OFF"
noclipToggle.TextColor3 = _G.NoclipEnabled and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
noclipToggle.TextScaled = true
noclipToggle.Font = Enum.Font.SourceSansBold
noclipToggle.Parent = noclipFrame

--== SOUL DETECTION SYSTEM ===================================================
local function findClosestSoul()
local closestPlayer, minDistance = nil, detectionRange
for _, v in pairs(Players:GetPlayers()) do
if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
local dist = (LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude
if dist <= minDistance then
minDistance = dist
closestPlayer = v
end
end
end
return closestPlayer
end

-- Follow Closest Soul
local following = false
local lastFollowUpdate = 0

local function followSoul()
if following then return end
following = true
RunService.Heartbeat:Connect(function()
if tick() - lastFollowUpdate < frameUpdateRate or _G.HitboxDisabled then return end
lastFollowUpdate = tick()

local closestSoul = findClosestSoul()
if closestSoul and closestSoul.Character and closestSoul.Character:FindFirstChild("HumanoidRootPart") then
hitboxLabel.Text = "Reaping: " .. closestSoul.Name
hitboxLabel.TextColor3 = Color3.fromRGB(255, 100, 100)

-- Animate skull when following
local rotationTween = TweenService:Create(hitboxSkull, TweenInfo.new(0.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {Rotation = 15})
rotationTween:Play()

local targetHRP = closestSoul.Character.HumanoidRootPart
for _, tool in pairs(LocalPlayer.Character:GetChildren()) do
if tool:IsA("Tool") then
local handle = tool:FindFirstChild("Handle")
if handle then
handle.Position = targetHRP.Position + Vector3.new(0, 2, 0)
elseif tool.PrimaryPart then
tool:SetPrimaryPartCFrame(targetHRP.CFrame * CFrame.new(0, 2, 0))
end
end
end

else
hitboxLabel.Text = "The Reaper watches..."
hitboxLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
hitboxSkull.Rotation = 0
end

end)

end

followSoul()

--== REAPER ESP + SOUL HITBOX SYSTEM =========================================
local lastESPUpdate = 0
RunService.RenderStepped:Connect(function()
if tick() - lastESPUpdate < frameUpdateRate or _G.HitboxDisabled then return end
lastESPUpdate = tick()

for _, v in pairs(Players:GetPlayers()) do
if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("Head") then
pcall(function()
local head = v.Character.Head
head.Size = Vector3.new(_G.HeadSize, _G.HeadSize, _G.HeadSize)
head.Transparency = 1
head.BrickColor = BrickColor.new("Really black")
head.Material = Enum.Material.ForceField
head.CanCollide = false
head.Massless = true

local soulHitbox = v.Character:FindFirstChild("SoulHitbox")
if not soulHitbox then
soulHitbox = Instance.new("Part")
soulHitbox.Name = "SoulHitbox"
soulHitbox.Size = _G.HitboxSize
soulHitbox.Transparency = 0.1
soulHitbox.BrickColor = BrickColor.new("Really black")
soulHitbox.Material = Enum.Material.ForceField
soulHitbox.Anchored = true
soulHitbox.CanCollide = false
soulHitbox.Massless = true
soulHitbox.Parent = v.Character

-- Dark glow effect
local selectionBox = Instance.new("SelectionBox")
selectionBox.Adornee = soulHitbox
selectionBox.Color3 = Color3.fromRGB(150, 0, 0)
selectionBox.LineThickness = 0.2
selectionBox.Transparency = 0.5
selectionBox.Parent = soulHitbox

-- Death Status Emoji
local deathEmoji = Instance.new("BillboardGui")
deathEmoji.Name = "DeathEmoji"
deathEmoji.Size = UDim2.new(8, 0, 8, 0)
deathEmoji.Adornee = soulHitbox
deathEmoji.AlwaysOnTop = true
deathEmoji.Parent = soulHitbox

local deathEmojiText = Instance.new("TextLabel")          
deathEmojiText.Name = "DeathEmojiLabel"          
deathEmojiText.Size = UDim2.new(1, 0, 1, 0)          
deathEmojiText.BackgroundTransparency = 1          
deathEmojiText.TextScaled = true          
deathEmojiText.Font = Enum.Font.GothamBlack          
deathEmojiText.TextColor3 = Color3.fromRGB(255, 255, 255)          
deathEmojiText.TextStrokeTransparency = 0          
deathEmojiText.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)          
deathEmojiText.Text = "👻"          
deathEmojiText.Parent = deathEmoji          

-- Soul Status Display          
local soulGui = Instance.new("BillboardGui")          
soulGui.Name = "SoulGui"          
soulGui.Size = UDim2.new(6, 0, 1.5, 0)          
soulGui.StudsOffset = Vector3.new(0, 4, 0)          
soulGui.Adornee = soulHitbox          
soulGui.AlwaysOnTop = true          
soulGui.Parent = soulHitbox          

local soulLabel = Instance.new("TextLabel")          
soulLabel.Name = "SoulLabel"          
soulLabel.Size = UDim2.new(1, 0, 1, 0)          
soulLabel.BackgroundTransparency = 1          
soulLabel.TextColor3 = Color3.fromRGB(255, 255, 255)          
soulLabel.TextStrokeTransparency = 0.3          
soulLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)          
soulLabel.TextScaled = true          
soulLabel.Font = Enum.Font.SourceSansBold          
soulLabel.Parent = soulGui          

-- Player name with gothic styling          
local nameGui = Instance.new("BillboardGui")          
nameGui.Name = "ReaperNameGui"          
nameGui.Size = UDim2.new(6, 0, 1, 0)          
nameGui.StudsOffset = Vector3.new(0, 6, 0)          
nameGui.Adornee = soulHitbox          
nameGui.AlwaysOnTop = true          
nameGui.Parent = soulHitbox          

local nameLabel = Instance.new("TextLabel")          
nameLabel.Name = "ReaperNameLabel"          
nameLabel.Size = UDim2.new(1, 0, 1, 0)          
nameLabel.BackgroundTransparency = 1          
nameLabel.TextColor3 = Color3.fromRGB(200, 200, 200)          
nameLabel.TextStrokeTransparency = 0.3          
nameLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)          
nameLabel.TextScaled = true          
nameLabel.Font = Enum.Font.SourceSansBold          
nameLabel.Text = "Soul of " .. v.Name          
nameLabel.Parent = nameGui

end

soulHitbox.CFrame = head.CFrame * CFrame.new(0, 2, 0)

local hum = v.Character:FindFirstChildOfClass("Humanoid")
if hum then
local hp = hum.Health
local maxHp = hum.MaxHealth
local deathEmoji = soulHitbox:FindFirstChild("DeathEmoji")
local soulGui = soulHitbox:FindFirstChild("SoulGui")
local selectionBox = soulHitbox:FindFirstChild("SelectionBox")

if deathEmoji and deathEmoji:FindFirstChild("DeathEmojiLabel") then          
    local e = deathEmoji.DeathEmojiLabel          
    if hp <= 0 then          
        e.Text = "⚰️"          
        e.TextColor3 = Color3.fromRGB(0, 0, 0)          
        if selectionBox then          
            selectionBox.Color3 = Color3.fromRGB(0, 0, 0)          
        end          
    elseif hp <= 25 then          
        e.Text = "💀"          
        e.TextColor3 = Color3.fromRGB(255, 0, 0)          
        if selectionBox then          
            selectionBox.Color3 = Color3.fromRGB(255, 0, 0)          
        end          
    elseif hp <= 50 then          
        e.Text = "☠️"          
        e.TextColor3 = Color3.fromRGB(255, 100, 0)          
        if selectionBox then          
            selectionBox.Color3 = Color3.fromRGB(255, 100, 0)          
        end          
    else          
        e.Text = "👻"          
        e.TextColor3 = Color3.fromRGB(200, 200, 200)          
        if selectionBox then          
            selectionBox.Color3 = Color3.fromRGB(150, 0, 0)          
        end          
    end          
end          

if soulGui and soulGui:FindFirstChild("SoulLabel") then          
    local soulPercent = (hp / maxHp) * 100          
    soulGui.SoulLabel.Text = string.format("Soul: %.0f%% (%.0f HP)", soulPercent, hp)          
              
    -- Color based on soul strength          
    if hp <= 25 then          
        soulGui.SoulLabel.TextColor3 = Color3.fromRGB(255, 0, 0)          
    elseif hp <= 50 then          
        soulGui.SoulLabel.TextColor3 = Color3.fromRGB(255, 165, 0)          
    else          
        soulGui.SoulLabel.TextColor3 = Color3.fromRGB(255, 255, 255)          
    end          
end

end

end)

end

end

end)

--== REAPER CONTROL SYSTEM ===================================================
UserInputService.InputBegan:Connect(function(input, processed)
if processed then return end

-- Hitbox Toggle
if input.KeyCode == _G.HitboxToggleKey then
_G.HitboxDisabled = not _G.HitboxDisabled

if _G.HitboxDisabled then
hitboxLabel.Text = "The Reaper rests..."
hitboxLabel.TextColor3 = Color3.fromRGB(100, 100, 100)
hitboxSkull.TextColor3 = Color3.fromRGB(100, 100, 100)
hitboxStroke.Color = Color3.fromRGB(50, 50, 50)
hitboxToggle.Text = "OFF"
hitboxToggle.TextColor3 = Color3.fromRGB(255, 0, 0)
else
hitboxLabel.Text = "The Reaper awakens..."
hitboxLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
hitboxSkull.TextColor3 = Color3.fromRGB(200, 0, 0)
hitboxStroke.Color = Color3.fromRGB(100, 0, 0)
hitboxToggle.Text = "ON"
hitboxToggle.TextColor3 = Color3.fromRGB(0, 255, 0)
end

end

-- Noclip Toggle
if input.KeyCode == _G.NoclipToggleKey then
_G.NoclipEnabled = not _G.NoclipEnabled

if _G.NoclipEnabled then
enableNoclip()
noclipLabel.Text = "Noclip: Active"
noclipLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
noclipGhost.TextColor3 = Color3.fromRGB(0, 255, 255)
noclipStroke.Color = Color3.fromRGB(0, 150, 150)
noclipToggle.Text = "ON"
noclipToggle.TextColor3 = Color3.fromRGB(0, 255, 0)
else
disableNoclip()
noclipLabel.Text = "Noclip: [N]"
noclipLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
noclipGhost.TextColor3 = Color3.fromRGB(100, 100, 100)
noclipStroke.Color = Color3.fromRGB(50, 50, 50)
noclipToggle.Text = "OFF"
noclipToggle.TextColor3 = Color3.fromRGB(255, 0, 0)
end

end

end)

--== CLEANUP SYSTEM ==========================================================
Players.PlayerRemoving:Connect(function(player)
if player.Character then
local soulHitbox = player.Character:FindFirstChild("SoulHitbox")
if soulHitbox then
soulHitbox:Destroy()
end
end
end)

-- Clean up on character respawn
LocalPlayer.CharacterAdded:Connect(function()
wait(1) -- Wait for character to fully load
if _G.NoclipEnabled then
enableNoclip()
end
end)

print("[ReaperToll] The Reaper's dominion is established... ⚰️💀👻") 
