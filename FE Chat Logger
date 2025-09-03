print("Welcome to wilsonchat%_ | Made by Wilsonation")

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")


local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ChatLoggerGui"
screenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0.15, 0, 0.2, 0) 
mainFrame.Position = UDim2.new(0.83, 0, 0.75, 0) 
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.BorderSizePixel = 1
mainFrame.Parent = screenGui


local titleBar = Instance.new("TextLabel")
titleBar.Size = UDim2.new(1, 0, 0, 20) 
titleBar.Position = UDim2.new(0, 0, 0, 0)
titleBar.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
titleBar.BorderSizePixel = 1
titleBar.Text = "wilsonchat%_"
titleBar.TextColor3 = Color3.fromRGB(255, 255, 255)
titleBar.Font = Enum.Font.SourceSansBold
titleBar.TextSize = 14 
titleBar.Parent = mainFrame


local clearButton = Instance.new("TextButton")
clearButton.Size = UDim2.new(0, 40, 0, 15) 
clearButton.Position = UDim2.new(1, -45, 0, 3) 
clearButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
clearButton.Text = "Clear"
clearButton.TextColor3 = Color3.fromRGB(255, 255, 255)
clearButton.Font = Enum.Font.SourceSansBold
clearButton.BorderSizePixel = 1
clearButton.TextSize = 12 
clearButton.Parent = titleBar

local scrollingFrame = Instance.new("ScrollingFrame")
scrollingFrame.Size = UDim2.new(1, -10, 1, -25) 
scrollingFrame.Position = UDim2.new(0, 5, 0, 22) 
scrollingFrame.BackgroundTransparency = 1
scrollingFrame.ScrollBarThickness = 3 
scrollingFrame.Parent = mainFrame


local uiListLayout = Instance.new("UIListLayout")
uiListLayout.SortOrder = Enum.SortOrder.LayoutOrder
uiListLayout.Padding = UDim.new(0, 3) 
uiListLayout.Parent = scrollingFrame

local function getTimestamp()
	return os.date("[%H:%M:%S]")
end

local function addChatMessage(player, message)
	local messageLabel = Instance.new("TextLabel")
	messageLabel.Size = UDim2.new(1, -10, 0, 15) 
	messageLabel.BackgroundTransparency = 1
	messageLabel.Text = string.format("%s %s: %s", getTimestamp(), player.Name, message)
	messageLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
	messageLabel.Font = Enum.Font.SourceSans
	messageLabel.TextSize = 13 
	messageLabel.TextXAlignment = Enum.TextXAlignment.Left
	messageLabel.TextWrapped = true 
	messageLabel.Parent = scrollingFrame

	
	scrollingFrame.CanvasSize = UDim2.new(0, 0, 0, uiListLayout.AbsoluteContentSize.Y + 10)
end


local function clearChatMessages()
	for _, child in pairs(scrollingFrame:GetChildren()) do
		if child:IsA("TextLabel") then
			child:Destroy()
		end
	end
	scrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
end

clearButton.MouseButton1Click:Connect(clearChatMessages)


local dragging = false
local dragStart = nil
local startPos = nil

titleBar.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = mainFrame.Position
	end
end)

UserInputService.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

RunService:BindToRenderStep("DragGUI", Enum.RenderPriority.Input.Value, function()
	if dragging then
		local mousePos = UserInputService:GetMouseLocation()
		local delta = mousePos - dragStart
		mainFrame.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)
		dragStart = mousePos
	end
end)

LocalPlayer.Chatted:Connect(function(message)
	addChatMessage(LocalPlayer, message)
end)


for _, player in pairs(Players:GetPlayers()) do
	if player ~= LocalPlayer then
		player.Chatted:Connect(function(message)
			addChatMessage(player, message)
		end)
	end
end

Players.PlayerAdded:Connect(function(player)
	player.Chatted:Connect(function(message)
		addChatMessage(player, message)
	end)
end)

screenGui.AncestryChanged:Connect(function()
	if not screenGui:IsDescendantOf(game) then
		RunService:UnbindFromRenderStep("DragGUI")
	end
end)
