local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer

local GREEN = Color3.fromRGB(0, 255, 80)
local FILL_TRANSPARENCY = 0.82
local OUTLINE_TRANSPARENCY = 0
local REFRESH_INTERVAL = 0.5

local highlights = {}
local playerConnections = {}

local function createHighlight(character)
	local highlight = Instance.new("Highlight")
	highlight.FillColor = GREEN
	highlight.FillTransparency = FILL_TRANSPARENCY
	highlight.OutlineColor = GREEN
	highlight.OutlineTransparency = OUTLINE_TRANSPARENCY
	highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
	highlight.Adornee = character
	highlight.Parent = character
	return highlight
end

local function removePlayer(player)
	if highlights[player] then
		highlights[player]:Destroy()
		highlights[player] = nil
	end
	
	if playerConnections[player] then
		for _, connection in ipairs(playerConnections[player]) do
			connection:Disconnect()
		end
		playerConnections[player] = nil
	end
end

local function onCharacterAdded(player, character)
	if player == localPlayer then
		return
	end
	
	if highlights[player] then
		highlights[player]:Destroy()
	end
	
	highlights[player] = createHighlight(character)
end

local function watchPlayer(player)
	if player == localPlayer then
		return
	end
	
	local connections = {}
	playerConnections[player] = connections
	
	table.insert(connections, player.CharacterAdded:Connect(function(character)
		onCharacterAdded(player, character)
	end))
	
	if player.Character then
		onCharacterAdded(player, player.Character)
	end
end

for _, player in ipairs(Players:GetPlayers()) do
	watchPlayer(player)
end

Players.PlayerAdded:Connect(function(player)
	watchPlayer(player)
end)

Players.PlayerRemoving:Connect(function(player)
	removePlayer(player)
end)
