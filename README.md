# Universal aimlock script
-- Coloque este LocalScript dentro de StarterPlayerScripts ou StarterGui

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local UserInput = game:GetService("UserInputService")
local Camera = workspace.CurrentCamera

local localPlayer = Players.LocalPlayer

-- === CRIAR GUI ===
local lockGui = Instance.new("ScreenGui")
lockGui.Name = "LockGui"
lockGui.ResetOnSpawn = false
lockGui.Parent = localPlayer:WaitForChild("PlayerGui")

-- Frame principal
local frame = Instance.new("Frame")
frame.Name = "Frame"
frame.Position = UDim2.new(0.405, 0, 0.425, 0)
frame.Size = UDim2.new(0, 263, 0, 110)
frame.BackgroundColor3 = Color3.new(1,1,1)
frame.Parent = lockGui

local frameCorner = Instance.new("UICorner")
frameCorner.CornerRadius = UDim.new(0,8)
frameCorner.Parent = frame

local frameStroke = Instance.new("UIStroke")
frameStroke.Color = Color3.new(0,0,0)
frameStroke.Thickness = 2
frameStroke.LineJoinMode = Enum.LineJoinMode.Round
frameStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual
frameStroke.Parent = frame

-- TextLabel "Aimlock Text"
local aimText = Instance.new("TextLabel")
aimText.Name = "Aimlock Text"
aimText.Size = UDim2.new(0, 160, 0, 32)
aimText.Position = UDim2.new(-0.004,0,0,0)
aimText.BackgroundColor3 = Color3.new(1,1,1)
aimText.BorderColor3 = Color3.new(0,0,0)
aimText.BackgroundTransparency = 1
aimText.Text = "Aimlock"
aimText.TextColor3 = Color3.new(0,0,0)
aimText.TextScaled = true
aimText.Font = Enum.Font.SourceSans
aimText.Parent = frame

local aimCorner = Instance.new("UICorner")
aimCorner.CornerRadius = UDim.new(0,8)
aimCorner.Parent = aimText

-- Toggle
local toggle = Instance.new("Frame")
toggle.Name = "Toggle"
toggle.Position = UDim2.new(0.639,0,0.055,0)
toggle.Size = UDim2.new(0,52,0,23)
toggle.BackgroundColor3 = Color3.fromRGB(62,62,62)
toggle.BorderColor3 = Color3.new(0,0,0)
toggle.Parent = frame

local toggleCorner = Instance.new("UICorner")
toggleCorner.CornerRadius = UDim.new(1,0)
toggleCorner.Parent = toggle

local knob = Instance.new("Frame")
knob.Name = "Knob"
knob.Position = UDim2.new(0.128,0,0.217,0)
knob.Size = UDim2.new(0,12,0,12)
knob.BackgroundColor3 = Color3.new(1,1,1)
knob.BorderColor3 = Color3.new(0,0,0)
knob.Parent = toggle

local knobCorner = Instance.new("UICorner")
knobCorner.CornerRadius = UDim.new(0,8)
knobCorner.Parent = knob

-- KeyToggle1
local keyToggle1 = Instance.new("Frame")
keyToggle1.Name = "KeyToggle1"
keyToggle1.Position = UDim2.new(0.509,0,0.53,0)
keyToggle1.Size = UDim2.new(0,100,0,22)
keyToggle1.BackgroundTransparency = 1
keyToggle1.Parent = lockGui

local keyButton = Instance.new("TextButton")
keyButton.Size = UDim2.new(0,24,0,22)
keyButton.Position = UDim2.new(0.25,0,-1.455,0)
keyButton.BackgroundColor3 = Color3.new(1,1,1)
keyButton.BorderColor3 = Color3.new(0,0,0)
keyButton.Text = "E"
keyButton.TextColor3 = Color3.new(0,0,0)
keyButton.TextScaled = true
keyButton.Font = Enum.Font.SourceSans
keyButton.RichText = false
keyButton.Parent = keyToggle1

local keyButtonCorner = Instance.new("UICorner")
keyButtonCorner.CornerRadius = UDim.new(0,8)
keyButtonCorner.Parent = keyButton

local keyButtonStroke = Instance.new("UIStroke")
keyButtonStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
keyButtonStroke.Thickness = 1
keyButtonStroke.Color = Color3.new(0,0,0)
keyButtonStroke.Parent = keyButton

local keyText = Instance.new("TextLabel")
keyText.Name = "Key text"
keyText.Size = UDim2.new(0,137,0,31)
keyText.Position = UDim2.new(-1.13,0,-1.657,0)
keyText.BackgroundColor3 = Color3.new(1,1,1)
keyText.BorderColor3 = Color3.new(0,0,0)
keyText.BackgroundTransparency = 1
keyText.Text = "Choose a key:"
keyText.TextColor3 = Color3.new(0,0,0)
keyText.TextScaled = true
keyText.Font = Enum.Font.SourceSans
keyText.RichText = false
keyText.Parent = keyToggle1

local keyTextCorner = Instance.new("UICorner")
keyTextCorner.CornerRadius = UDim.new(1,0)
keyTextCorner.Parent = keyText

-- === AIMLOCK ===
local mirando = false
local teclaAtual = Enum.KeyCode.E
local escolhendoTecla = false

keyButton.Text = teclaAtual.Name

local posOff = UDim2.new(0.128,0,0.217,0)
local posOn = UDim2.new(0.648,0,0.217,0)
local corOff = Color3.fromRGB(70,70,70)
local corOn = Color3.fromRGB(0,255,140)

local function atualizarToggle()
	if mirando then
		TweenService:Create(knob,TweenInfo.new(0.25),{Position=posOn}):Play()
		TweenService:Create(toggle,TweenInfo.new(0.25),{BackgroundColor3=corOn}):Play()
	else
		TweenService:Create(knob,TweenInfo.new(0.25),{Position=posOff}):Play()
		TweenService:Create(toggle,TweenInfo.new(0.25),{BackgroundColor3=corOff}):Play()
	end
end

toggle.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		mirando = not mirando
		atualizarToggle()
	end
end)

keyButton.MouseButton1Click:Connect(function()
	escolhendoTecla = true
	keyButton.Text = "..."
end)

UserInput.InputBegan:Connect(function(input,gameProcessed)
	if gameProcessed then return end

	if escolhendoTecla then
		if input.UserInputType == Enum.UserInputType.Keyboard then
			teclaAtual = input.KeyCode
			keyButton.Text = teclaAtual.Name
			escolhendoTecla = false
		end
		return
	end

	if input.KeyCode == teclaAtual then
		mirando = not mirando
		atualizarToggle()
	end
end)

local function getClosestHeadToCursor()
	local closestHead = nil
	local shortestDistance = math.huge
	local mousePos = UserInput:GetMouseLocation()

	for _, player in pairs(Players:GetPlayers()) do
		if player ~= localPlayer and player.Character and player.Character:FindFirstChild("Head") then
			local headPos = Camera:WorldToViewportPoint(player.Character.Head.Position)
			if headPos.Z > 0 then
				local dist = (Vector2.new(headPos.X, headPos.Y)-Vector2.new(mousePos.X,mousePos.Y)).Magnitude
				if dist < shortestDistance then
					shortestDistance = dist
					closestHead = player.Character.Head
				end
			end
		end
	end

	for _, model in pairs(workspace:GetChildren()) do
		if model:IsA("Model") and model:FindFirstChild("Head") and model ~= localPlayer.Character then
			local headPos = Camera:WorldToViewportPoint(model.Head.Position)
			if headPos.Z > 0 then
				local dist = (Vector2.new(headPos.X,headPos.Y)-Vector2.new(mousePos.X,mousePos.Y)).Magnitude
				if dist < shortestDistance then
					shortestDistance = dist
					closestHead = model.Head
				end
			end
		end
	end

	return closestHead
end

RunService.RenderStepped:Connect(function()
	if mirando then
		local targetHead = getClosestHeadToCursor()
		if targetHead then
			Camera.CFrame = CFrame.new(Camera.CFrame.Position,targetHead.Position)
		end
	end
end)

-- === DRAG DA GUI ===
local function makeGuiDraggable()
	local dragging = false
	local dragInput, mousePos, framePos
	local frameOffset = keyToggle1.Position - frame.Position

	local function update(input)
		local delta = input.Position - mousePos
		local newFramePos = framePos + UDim2.new(0,delta.X,0,delta.Y)
		local screenSize = Camera.ViewportSize
		local frameSize = frame.AbsoluteSize

		newFramePos = UDim2.new(
			0, math.clamp(newFramePos.X.Offset,0,screenSize.X-frameSize.X),
			0, math.clamp(newFramePos.Y.Offset,0,screenSize.Y-frameSize.Y)
		)

		TweenService:Create(frame,TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),{Position=newFramePos}):Play()
		local newKeyPos = newFramePos + frameOffset
		TweenService:Create(keyToggle1,TweenInfo.new(0.1,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),{Position=newKeyPos}):Play()
	end

	local function beginDrag(input)
		dragging = true
		mousePos = input.Position
		framePos = frame.Position
		frameOffset = keyToggle1.Position - frame.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end

	frame.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 then
			beginDrag(input)
		end
	end)
	keyToggle1.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 then
			beginDrag(input)
		end
	end)
	for _, child in pairs(keyToggle1:GetDescendants()) do
		if child:IsA("GuiButton") then
			child.InputBegan:Connect(function(input)
				if input.UserInputType == Enum.UserInputType.MouseButton1 then
					beginDrag(input)
				end
			end)
		end
	end

	frame.InputChanged:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseMovement then
			dragInput = input
		end
	end)
	UserInput.InputChanged:Connect(function(input)
		if input == dragInput and dragging then
			update(input)
		end
	end)
end

makeGuiDraggable()
