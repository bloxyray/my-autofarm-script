--// @ray
if (not game:IsLoaded()) then 
	game.Loaded:Wait()
	task.wait(1)
end

repeat task.wait(0.1) until (game:GetService("Players").LocalPlayer) and (game:GetService("Players").LocalPlayer.Character)

local SG = Instance.new("ScreenGui")
SG.Parent = game:GetService("CoreGui")
SG.Name = "abcdefg"
SG.IgnoreGuiInset = true 
local TL = Instance.new("TextLabel")
TL.Parent = SG 
TL.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
TL.Active = false
TL.Font = Enum.Font.MontserratMedium
TL.TextColor3 = Color3.fromRGB(200, 200, 200)
TL.TextSize = 24
TL.AnchorPoint = Vector2.new(0.5, 0.5)
TL.Position = UDim2.new(0.5, 0, 0.5, 0)
TL.Size = UDim2.new(1, 0, 1, 0)


local Player = game:GetService("Players").LocalPlayer
local Cashiers = workspace.Cashiers 
local Drop = workspace.Ignored.Drop
local Dis = false
local Broken = 0 
local StartTick = os.time()

_G.Disable = function()
	Dis = true
	game:GetService("RunService"):Set3dRenderingEnabled(true)
	setfpscap(60)
	game:GetService("CoreGui").abcdefg:Destroy()
end

Player.DevCameraOcclusionMode = Enum.DevCameraOcclusionMode.Invisicam
Player.CameraMaxZoomDistance = 6
Player.CameraMinZoomDistance = 6

TL.Text = "VERY OP FARM - @ray\n@"..Player.Name.."\n$999,999,999"

pcall(function()local a=game:GetService("ReplicatedStorage").MainEvent;local b={"CHECKER_1","TeleportDetect","OneMoreTime"}local c;c=hookmetamethod(game,"__namecall",function(...)local d={...}local self=d[1]local e=getnamecallmethod()if e=="FireServer"and self==a and table.find(b,d[2])then return end return c(...)end)end)

local Click = function(Part)
	local Input = game:GetService("VirtualInputManager")
	local Pos = workspace.Camera:WorldToScreenPoint(Part.Position)
	local T = os.time()

	if (Part:GetAttribute("OriginalPos") == nil) then 
		Part:SetAttribute("OriginalPos", Part.Position)
	end

	repeat 
		Part.CFrame = (workspace.Camera.CFrame + workspace.Camera.CFrame.LookVector * 1) * CFrame.Angles(90, 0, 0)
		Input:SendMouseButtonEvent(workspace.Camera.ViewportSize.X/2, workspace.Camera.ViewportSize.Y/2, 0, true, game, 1)
		task.wait()
		Input:SendMouseButtonEvent(workspace.Camera.ViewportSize.X/2, workspace.Camera.ViewportSize.Y/2, 0, false, game, 1)
	until (Part == nil) or (Part:FindFirstChild("ClickDetector") == nil) or (os.time()-T>=2)
end

local AntiSit = function(Char)
	task.wait(1)	
	local Hum = Char:WaitForChild("Humanoid")
	Hum.Seated:Connect(function()
		warn("SITTING")
		Hum.Sit = false
		Hum:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
		task.wait(0.3)
		Hum.Jump = true
	end)
end

local GetCash = function()
	local Found = {}
	
	for i,v in pairs(Drop:GetChildren()) do 
		if (v.Name == "MoneyDrop") then 
			local Pos = nil 
			
			if (v:GetAttribute("OriginalPos") ~= nil) then 
				Pos = v:GetAttribute("OriginalPos")
			else 
				Pos = v.Position
			end
			if (Pos - Player.Character.HumanoidRootPart.Position).Magnitude <= 17 then 
				Found[#Found+1] = v 
			end
		end
	end
	return Found
end

local GetCashier = function()
	for i,v in pairs(Cashiers:GetChildren()) do 
		if (i == 15) and (Vector3.new(-625, 10, -286) - v.Head.Position).Magnitude <= 20 then 
			v:MoveTo(Vector3.new(-622.948, 24, -286.52))
			for x,z in pairs(v:GetChildren()) do 
				if (z:IsA("Part")) or (z:IsA("BasePart")) then 
					z.CanCollide = false 
				end
			end
		elseif (i == 16) and (Vector3.new(-625, 10, -286) - v.Head.Position).Magnitude <= 20 then
			v:MoveTo(Vector3.new(-629.948, 24, -286.52))
			for x,z in pairs(v:GetChildren()) do 
				if (z:IsA("Part")) or (z:IsA("BasePart")) then 
					z.CanCollide = false 
				end
			end
		end
		
		if (v.Humanoid.Health > 0) then 
			return v 
		end
	end
	return nil
end

local To = function(CF)
	Player.Character.HumanoidRootPart.CFrame = CF 
	Player.Character.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
end

task.spawn(function()
	while true and task.wait() do 
		if (Player.Character == nil) or (Player.Character:FindFirstChild("FULLY_LOADED_CHAR") == nil) or (Dis == true) then 
			return print("NO")
		end
		
		local Cashier = nil
		repeat 
			Cashier = GetCashier()

			if (Player.Backpack:FindFirstChild("Combat") ~= nil) then 
				Player.Backpack.Combat.Parent = Player.Character 
			end

			task.wait()
		until (Cashier ~= nil)
		
		repeat 
			To( (Cashier.Head.CFrame+Vector3.new(0, -2.5, 0)) * CFrame.Angles(math.rad(90), 0, 0) ) 
			task.wait()
			Player.Character.Combat:Activate()
		until (Cashier.Humanoid.Health <= 0)
		Broken += 1

		To(Cashier.Head.CFrame + Cashier.Head.CFrame.LookVector * Vector3.new(0, 2, 0))

		for i,v in pairs(Player.Character:GetChildren()) do 
			if (v:IsA("Tool")) then 
				v.Parent = Player.Backpack 
			end
		end
		
		local Cash = GetCash()
		
		for i,v in pairs(Cash) do 
			Click(v)
		end
	end
end)

local StartCash = Player.DataFolder.Currency.Value
task.spawn(function()
	while true and task.wait(0.5) do 
		print(TL.Text)
		TL.Text = "VERY OP AUTOFARM - @ray\n@"..Player.Name.."\n$"..tostring(Player.DataFolder.Currency.Value):reverse():gsub("...","%0,",math.floor((#tostring(Player.DataFolder.Currency.Value)-1)/3)):reverse().."\nATMS: "..tostring(Broken).."\n"..string.format("%02i:%02i:%02i", (os.time()-StartTick)/60^2, (os.time()-StartTick)/60%60, (os.time()-StartTick)%60).."\nProfit: $"..tostring(Player.DataFolder.Currency.Value-StartCash):reverse():gsub("...","%0,",math.floor((#tostring(Player.DataFolder.Currency.Value-StartCash)-1)/3)):reverse().."\n"..tostring(GetCashier()).."\nthanks for using my script!"
	end
end)


Player.Idled:Connect(function()
	for i = 1, 10 do 
		game:GetService("VirtualUser"):Button2Down(Vector2.new(0, 0), workspace.CurrentCamera.CFrame) 
		task.wait(0.2) 
		game:GetService("VirtualUser"):Button2Up(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
		task.wait(0.2)
	end
end)

pcall(function() UserSettings().GameSettings.MasterVolume = 0 end)
pcall(function() settings().Rendering.QualityLevel = "Level01" end)

Player.CharacterAdded:Connect(AntiSit)
task.spawn(function()
	task.wait(3)
	AntiSit(Player.Character)
end)

for i = 1, 10 do 
	setfpscap(_G.AutofarmSettings.Fps)
	task.wait(0.1)
end

if (_G.AutofarmSettings.Saver == true) then 
	game:GetService("RunService"):Set3dRenderingEnabled(false) 
else 
	SG.Enabled = false
end
