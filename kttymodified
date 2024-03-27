repeat task.wait(1) until game.PlaceId ~= nil
repeat task.wait(1) until game:GetService("Players") and game:GetService("Players").LocalPlayer
repeat task.wait(1) until not game.Players.LocalPlayer.PlayerGui:FindFirstChild("__INTRO")
if game:IsLoaded() and getgenv().MoneyPrinter.maybeCPUReducer then
	pcall(function()
		for _, v in pairs(game:GetService("Workspace"):FindFirstChild("__THINGS"):GetChildren()) do
			if table.find({"ShinyRelics", "Ornaments", "Instances", "Ski Chairs"}, v.Name) then
				v:Destroy()
			end
		end

		for _, v in pairs(game:GetService("Workspace"):FindFirstChild("__THINGS").__INSTANCE_CONTAINER.Active.AdvancedFishing:GetChildren()) do
			if string.find(v.Name, "Model") or string.find(v.Name, "Water") or string.find(v.Name, "Debris") or string.find(v.Name, "Interactable") then
				v:Destroy()
			end

			if v.Name == "Map" then
				for _, v in pairs(v:GetChildren()) do
					if v.Name ~= "Union" then
						v:Destroy()
					end
				end
			end
		end

		game:GetService("Workspace"):WaitForChild("ALWAYS_RENDERING"):Destroy()
	end)

	local Workspace = game:GetService("Workspace")
	local Terrain = Workspace:WaitForChild("Terrain")
	Terrain.WaterReflectance = 0
	Terrain.WaterTransparency = 1
	Terrain.WaterWaveSize = 0
	Terrain.WaterWaveSpeed = 0

	local Lighting = game:GetService("Lighting")
	Lighting.Brightness = 0
	Lighting.GlobalShadows = false
	Lighting.FogEnd = 9e100
	Lighting.FogStart = 0

	sethiddenproperty(Lighting, "Technology", 2)
	sethiddenproperty(Terrain, "Decoration", false)

	local function clearTextures(v)
		if v:IsA("BasePart") and not v:IsA("MeshPart") then
			v.Material = "Plastic"
			v.Reflectance = 0
			v.Transparency = 1
		elseif (v:IsA("Decal") or v:IsA("Texture")) then
			v.Transparency = 1
		elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then
			v.Lifetime = NumberRange.new(0)
		elseif v:IsA("Explosion") then
			v.BlastPressure = 1
			v.BlastRadius = 1
		elseif v:IsA("Fire") or v:IsA("SpotLight") or v:IsA("Smoke") or v:IsA("Sparkles") then
			v.Enabled = false
		elseif v:IsA("MeshPart") then
			v.Material = "Plastic"
			v.Reflectance = 0
			v.TextureID = 10385902758728957
		elseif v:IsA("SpecialMesh")  then
			v.TextureId = 0
		elseif v:IsA("ShirtGraphic") then
			v.Graphic = 1
		elseif (v:IsA("Shirt") or v:IsA("Pants")) then
			v[v.ClassName .. "Template"] = 1
		elseif v.Name == "Foilage" and v:IsA("Folder") then
			v:Destroy()
		elseif string.find(v.Name, "Tree") or string.find(v.Name, "Water") or string.find(v.Name, "Bush") or string.find(v.Name, "grass") then
			task.wait()
			v:Destroy()
		end
	end

	game:GetService("Lighting"):ClearAllChildren()

	for _, v in pairs(Workspace:GetDescendants()) do
		clearTextures(v)
	end

	Workspace.DescendantAdded:Connect(function(v)
		clearTextures(v)
	end)

	for _, v in pairs(game.Players:GetChildren()) do
		for _, v2 in pairs(v.Character:GetDescendants()) do
			if v2:IsA("BasePart") or v2:IsA("Decal") then
				v2.Transparency = 1
			end
		end
	end

	game.Players.PlayerAdded:Connect(function(player)
		player.CharacterAdded:Connect(function(character)
			for _, v in pairs(character:GetDescendants()) do
				if v:IsA("BasePart") or v:IsA("Decal") then
					v.Transparency = 1
				end
			end
		end)
	end)

	for i,v in pairs(game.Players.LocalPlayer.PlayerGui:GetChildren()) do
		if v:IsA("ScreenGui") then
			v.Enabled = false
		end
	end

	for i, v in pairs(game:GetService("StarterGui"):GetChildren()) do
		if v:IsA("ScreenGui") then
			v.Enabled = false
		end
	end

	for i, v in pairs(game:GetService("CoreGui"):GetChildren()) do
		if v:IsA("ScreenGui") then
			v.Enabled = false
		end
	end
	setfpscap(8)
end	
local LargeRAP = 11000; local SmallRAP = 2800
local RunService = game:GetService("RunService")
local HttpService = game:GetService("HttpService")
local Player = game:GetService("Players").LocalPlayer
local RepStor = game:GetService("ReplicatedStorage")
local Library = require(RepStor.Library)
local HRP = Player.Character.HumanoidRootPart
local saveMod = require(RepStor.Library.Client.Save)
local BreakMod = require(RepStor.Library.Client.BreakableCmds)
local Slingshot = getsenv(Player.PlayerScripts.Scripts.Game.Misc.Slingshot)
local RAPValues = getupvalues(require(RepStor.Library).DevRAPCmds.Get)[1]
hookfunction(require(game.ReplicatedStorage.Library.Client.PlayerPet).CalculateSpeedMultiplier, function() return 250 end)
function getInfo(name) return saveMod.Get()[name] end 
function getTool() return Player.Character:FindFirstChild("WEAPON_"..Player.Name, true) end
function equipTool(toolName) return Library.Network.Invoke(toolName.."_Toggle") end
function getCurrentZone() return Library["MapCmds"].GetCurrentZone() end
function sendNotif(msg)
	local message = {content = msg}
	local jsonMessage = HttpService:JSONEncode(message)
	local success, webMessage = pcall(function() 
		HttpService:PostAsync(getgenv().MoneyPrinter.webURL, jsonMessage) 
	end)
	if not success then 
		local response = request({Url = getgenv().MoneyPrinter.webURL,Method = "POST",Headers = {["Content-Type"] = "application/json"},Body = jsonMessage})
	end
end
function getBalloonUID(zoneName) 
	for i,v in pairs(BreakMod.AllByZoneAndClass(zoneName, "Chest")) do
		if v:GetAttribute("OwnerUsername") == Player.Name and string.find(v:GetAttribute("BreakableID"), "Balloon Gift") then
			return v:GetAttribute("BreakableUID")
		elseif v:GetAttribute("OwnerUserName") ~= Player.Name and string.find(v:GetAttribute("BreakableID"), "Balloon Gift") then
			return "Skip"
		end
	end
end
function getServer()
	local servers = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. tostring(game.PlaceId) .. '/servers/Public?sortOrder=Asc&limit=100')).data
	local server = servers[Random.new():NextInteger(1, 100)]
	if server then return server else return getServer() end
end
function getPresents() for i,v in pairs(Library.Save.Get().HiddenPresents) do 
		if not v.Found and v.ID then 
			local success,reason = Library.Network.Invoke("Hidden Presents: Found", v.ID) 
		end 
	end 
end
function getTotalRAP(num)
	local suffixes = {"", "k", "m", "b"}
	local suffixInd = 1
	while num >= 1000 and suffixInd < #suffixes do
		num = num / 1000
		suffixInd = suffixInd + 1
	end
	local formattedNum
	if num % 1 == 0 then
		formattedNum = string.format("%d", num)
	else
		formattedNum = string.format("%.1f", num):gsub("%.?0+$", "")
	end
	return formattedNum .. suffixes[suffixInd]
end
-- AUTO ORB
local autoOrbConnection = nil
local autoLootBagConnection = nil
for i, v in workspace.__THINGS.Orbs:GetChildren() do
	Library.Network.Fire("Orbs: Collect",{tonumber(v.Name)})
	Library.Network.Fire("Orbs_ClaimMultiple",{[1]={[1]=v.Name}})
	task.wait()
	v:Destroy()
end
for i, v in workspace.__THINGS.Lootbags:GetChildren() do
	Library.Network.Fire("Lootbags_Claim",{v.Name})
	task.wait()
	v:Destroy()
end
autoOrbConnection = workspace.__THINGS.Orbs.ChildAdded:Connect(function(v)
	Library.Network.Fire("Orbs: Collect",{tonumber(v.Name)})
	Library.Network.Fire("Orbs_ClaimMultiple",{[1]={[1]=v.Name}})
	task.wait()
	v:Destroy()
end)
autoLootBagConnection = workspace.__THINGS.Lootbags.ChildAdded:Connect(function(v)
	Library.Network.Fire("Lootbags_Claim",{v.Name})
	task.wait()
	v:Destroy()
end)
local startBalloons = #workspace.__THINGS.BalloonGifts:GetChildren()
if #workspace.__THINGS.BalloonGifts:GetChildren() <= 1 then
	repeat game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, getServer().id, Player) task.wait(3) until not game.PlaceId
end
local startGifts = 0
local startLarge = 0
for i,v in pairs(getInfo("Inventory").Misc) do
	if startGifts ~= 0 and startLarge ~= 0 then break end
	if v.id == "Gift Bag" then
		startGifts = (v._am or 1)
	elseif v.id == "Large Gift Bag" then
		startLarge = (v._am or 1)
	end
end
local startTime = os.time()
while getgenv().MoneyPrinter.autoBalloons do task.wait()
	if getgenv().MoneyPrinter.autoPresents then getPresents() end
	for _,Balloon in pairs(Library.Network.Invoke("BalloonGifts_GetActiveBalloons")) do task.wait(0.03)
		if Balloon.Id then
			while Library.Network.Invoke("BalloonGifts_GetActiveBalloons")[Balloon.Id] do task.wait(0.03)
				if not getTool() then equipTool(getgenv().MoneyPrinter.toolName) end
				local breakableId = getBalloonUID(getCurrentZone())
				if breakableId == "Skip" then break end
				if breakableId then
					HRP.CFrame = CFrame.new(Balloon.LandPosition)
					Library.Network.Fire("Breakables_PlayerDealDamage", breakableId)
				elseif not Balloon.Popped then
					HRP.CFrame = CFrame.new(Balloon.Position + Vector3.new(0,30,0))
					Slingshot.fireWeapon()
					Library.Network.Fire("BalloonGifts_BalloonHit", Balloon.Id)
				end
			end
		end
	end
	local currentTime = os.time()
	if getgenv().MoneyPrinter.serverHopper then
		if not getgenv().MoneyPrinter.avoidCooldown or (getgenv().MoneyPrinter.avoidCooldown and currentTime - startTime >= getgenv().MoneyPrinter.minServerTime) then
			local endGifts = 0
			local endLarge = 0 
			for i,v in pairs(getInfo("Inventory").Misc) do
				if endGifts ~= 0 and endLarge ~= 0 then break end
				if v.id == "Gift Bag" then
					endGifts = (v._am or 1)
				elseif v.id == "Large Gift Bag" then
					endLarge = (v._am or 1)
				end
			end
			if getgenv().MoneyPrinter.sendWeb then
				sendNotif("```asciidoc\n[ "..Player.Name.." Earned ]\n‐ "..tostring(endGifts - startGifts).." Small :: "..tostring(getTotalRAP((endGifts - startGifts) * SmallRAP)).." \n‐ "..tostring(endLarge - startLarge).." Large :: "..tostring(getTotalRAP((endLarge - startLarge) * LargeRAP)).." \n\n[ Total / Server ]\n‐ "..tostring(endGifts).." Small :: "..tostring(getTotalRAP(endGifts * SmallRAP)).." \n‐ "..tostring(endLarge).." Large :: "..tostring(getTotalRAP(endLarge * LargeRAP)).." \n- took "..tostring(currentTime - startTime).." seconds \n- had "..tostring(startBalloons).." balloons\n```")
			end
			repeat game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, getServer().id, Player) task.wait(3) until not game.PlaceId
		end
	end
end
