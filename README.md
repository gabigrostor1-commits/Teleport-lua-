-- Services
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "TeleportGui"
gui.Parent = player:WaitForChild("PlayerGui")

local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 160, 0, 50)
button.Position = UDim2.new(0, 20, 0, 200)
button.Text = "Teleport to Base"
button.Parent = gui

-- Base part
local basePart = workspace:WaitForChild("BaseTeleport")

-- Teleport function
button.MouseButton1Click:Connect(function()
	local character = player.Character or player.CharacterAdded:Wait()
	local hrp = character:WaitForChild("HumanoidRootPart")
	local humanoid = character:WaitForChild("Humanoid")

	-- Save equipped tool
	local tool = character:FindFirstChildOfClass("Tool")

	-- Disable collisions (go through walls)
	for _, part in pairs(character:GetDescendants()) do
		if part:IsA("BasePart") then
			part.CanCollide = false
		end
	end

	-- Teleport
	hrp.CFrame = basePart.CFrame + Vector3.new(0, 5, 0)

	-- Re-equip tool
	if tool then
		humanoid:EquipTool(tool)
	end

	-- Re-enable collisions after short delay
	task.delay(1, function()
		for _, part in pairs(character:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = true
			end
		end
	end)
end)
