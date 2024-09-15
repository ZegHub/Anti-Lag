-- Eğer daha önce bir GUI varsa, onu yok et
if game.Players.LocalPlayer:FindFirstChild("PlayerGui"):FindFirstChild("CustomGUI") then
game.Players.LocalPlayer.PlayerGui.CustomGUI:Destroy()
end

local screenGui = Instance.new("ScreenGui")
local frame = Instance.new("Frame")
local button = Instance.new("TextButton")
local closeButton = Instance.new("TextButton")

-- Yeni GUI'yi oluştur ve bir isim ver (CustomGUI)
screenGui.Name = "CustomGUI"
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false

frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0.5, -100, 0.5, -50)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.Parent = screenGui

button.Size = UDim2.new(1, -20, 1, -20)
button.Position = UDim2.new(0, 10, 0, 10)
button.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.Text = "Toggle 3D Rendering"
button.Parent = frame

local is3DRenderingEnabled = true

button.MouseButton1Click:Connect(function()
is3DRenderingEnabled = not is3DRenderingEnabled
game:GetService("RunService"):Set3dRenderingEnabled(is3DRenderingEnabled)
button.Text = is3DRenderingEnabled and "Turn 3D Rendering Off" or "Turn 3D Rendering On"
end)

closeButton.Size = UDim2.new(0, 20, 0, 20)
closeButton.Position = UDim2.new(1, -25, 0, 5)
closeButton.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.Text = "✖"
closeButton.Parent = frame

closeButton.MouseButton1Click:Connect(function()
screenGui:Destroy()
end)

local dragToggle = nil
local dragSpeed = 0.2
local dragInput = nil
local dragStart = nil
local startPos = nil

local function update(input)
local delta = input.Position - dragStart
frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

frame.InputBegan:Connect(function(input)
if input.UserInputType == Enum.UserInputType.MouseButton1 then
dragToggle = true
dragStart = input.Position
startPos = frame.Position
input.Changed:Connect(function()
if input.UserInputState == Enum.UserInputState.End then
dragToggle = false
end
end)
end
end)

frame.InputChanged:Connect(function(input)
if input.UserInputType == Enum.UserInputType.MouseMovement then
dragInput = input
end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
if input == dragInput and dragToggle then
update(input)
end
end)
