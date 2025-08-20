-- Script đặt trong ServerScriptService, tên file: menu

local portal1 = workspace:WaitForChild("Portal1")
local portal2 = workspace:WaitForChild("Portal2")
local cooldownTime = 2 -- thời gian chờ giữa các lần dịch chuyển

-- Tạo bảng cooldown cho từng người chơi
local playerCooldowns = {}

-- Hàm kiểm tra và dịch chuyển
local function teleportPlayer(hit, destinationPortal)
	if not hit.Parent then return end

	local humanoid = hit.Parent:FindFirstChild("Humanoid")
	if not humanoid then return end

	local player = game.Players:GetPlayerFromCharacter(hit.Parent)
	if not player then return end

	-- Kiểm tra cooldown
	if playerCooldowns[player] and tick() - playerCooldowns[player] < cooldownTime then
		return
	end

	-- Dịch chuyển
	local character = player.Character
	if character and character:FindFirstChild("HumanoidRootPart") then
		character.HumanoidRootPart.CFrame = destinationPortal.CFrame + Vector3.new(0, 3, 0)
		playerCooldowns[player] = tick()
	end
end

-- Kết nối sự kiện chạm
portal1.Touched:Connect(function(hit)
	teleportPlayer(hit, portal2)
end)

portal2.Touched:Connect(function(hit)
	teleportPlayer(hit, portal1)
end)
