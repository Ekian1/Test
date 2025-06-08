-- Auto grab all tools in the game for private server testing
local player = game:GetService("Players").LocalPlayer
local backpack = player:WaitForChild("Backpack")

-- List of services to scan
local services = {
	game:GetService("Workspace"),
	game:GetService("ReplicatedStorage"),
	game:GetService("StarterPack"),
	game:GetService("Lighting"),
	game:GetService("StarterGui"),
	game:GetService("Players"),
	game:GetService("ServerStorage"),
	game:GetService("ReplicatedFirst")
}

-- Recursive function to find tools
local function findTools(container)
	for _, obj in ipairs(container:GetDescendants()) do
		if obj:IsA("Tool") then
			local success, clone = pcall(function()
				return obj:Clone()
			end)
			if success and clone then
				clone.Parent = backpack
			end
		end
	end
end

-- Scan all services
for _, service in ipairs(services) do
	pcall(function()
		findTools(service)
	end)
end

-- Optional notification
game.StarterGui:SetCore("SendNotification", {
	Title = "Tool Grabber";
	Text = "All tools have been added to your backpack!";
	Duration = 4;
})
