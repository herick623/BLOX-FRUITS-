-- CONFIG LEVE 2800 - SÓ SEA EVENT + V4 + LEVIATHAN
-- Cola e deixa AFK

getgenv().Config2800 = {
    SeaFarm = true,
    FastAttack = true,
    FastAttackSpeed = 0.01, -- 0 delay
    UseFruitM1 = true, -- true = fruta / false = melee/gun
    GunDistance = 60,
    
    -- V4
    AutoMirage = true,
    AutoHopMirage = true,
    AutoGear = true,
    AutoTrial = true,
    AutoV4 = true,
    
    -- SEA
    Targets = {"Leviathan", "Terrorshark", "Shark", "Piranha", "Ghost Ship", "Sea Beast", "Fish Crew"},
    Bring = true, -- trava o boss
    SharkAnchor = true,
    AnchorAtHP = 20, -- usa anchor com 20% de vida
    AutoCollectLoot = true,
    
    -- Anti lag celular
    WhiteScreen = true,
    AntiAFK = true,
    FPSBoost = true
}

-- FAST ATTACK ULTRA
task.spawn(function()
    while true do task.wait(0.01)
        pcall(function()
            local ac = require(game.Players.LocalPlayer.PlayerScripts.CombatFramework).activeController
            if ac then
                ac.timeToNextAttack = 0
                ac.hitboxMagnitude = 150
                ac.increment = 4
                ac.blocking = false
                ac:attack()
            end
        end)
    end
end)

-- FARM LOOP
local plr = game.Players.LocalPlayer

function GetBoss()
    for _,v in pairs(workspace.Enemies:GetChildren()) do
        if v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
            local n = v.Name:lower()
            if n:find("leviathan") or n:find("terror") or n:find("shark") or n:find("piranha") or n:find("beast") or n:find("ghost") then
                return v
            end
        end
    end
    if workspace:FindFirstChild("SeaBeasts") then
        for _,v in pairs(workspace.SeaBeasts:GetChildren()) do
            if v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then return v end
        end
    end
end

function Equip()
    local c = plr.Character
    if c:FindFirstChildOfClass("Tool") then return c:FindFirstChildOfClass("Tool") end
    for _,t in pairs(plr.Backpack:GetChildren()) do
        if getgenv().Config2800.UseFruitM1 and t.ToolTip == "Blox Fruit" then c.Humanoid:EquipTool(t) return t end
    end
    for _,t in pairs(plr.Backpack:GetChildren()) do if t:IsA("Tool") then c.Humanoid:EquipTool(t) return t end end
end

function DoSeaFarm()
    while getgenv().Config2800.SeaFarm do
        task.wait()
        local boss = GetBoss()
        if boss and boss:FindFirstChild("HumanoidRootPart") then
            -- trava boss
            if getgenv().Config2800.Bring then
                pcall(function()
                    boss.HumanoidRootPart.CanCollide = false
                    boss.HumanoidRootPart.Size = Vector3.new(90,90,90)
                    boss.Humanoid.WalkSpeed = 0
                end)
            end
            
            repeat task.wait(0.05)
                pcall(function()
                    local dist = getgenv().Config2800.UseFruitM1 and 8 or getgenv().Config2800.GunDistance
                    plr.Character.HumanoidRootPart.CFrame = boss.HumanoidRootPart.CFrame * CFrame.new(0,12,dist)
                    Equip():Activate()
                    game.ReplicatedStorage.Remotes.CommF_:InvokeServer("Attack")
                    
                    -- SHARK ANCHOR AUTO
                    if getgenv().Config2800.SharkAnchor and boss.Humanoid.Health / boss.Humanoid.MaxHealth <= getgenv().Config2800.AnchorAtHP/100 then
                        game.ReplicatedStorage.Remotes.CommF_:InvokeServer("UseSharkAnchor")
                    end
                end)
            until boss.Humanoid.Health <= 0 or not boss.Parent
            
            -- coleta loot
            if getgenv().Config2800.AutoCollectLoot then
                task.wait(0.5)
                for _,v in pairs(workspace:GetChildren()) do
                    if v:FindFirstChild("Handle") and (v.Name:lower():find("anchor") or v.Name:lower():find("shark")) then
                        plr.Character.HumanoidRootPart.CFrame = v.Handle.CFrame
                        task.wait(0.2)
                    end
                end
            end
        else
            -- sem boss? procura Mirage ou patrulha Leviathan
            if getgenv().Config2800.AutoMirage and not workspace:FindFirstChild("Mirage Island") then
                plr.Character.HumanoidRootPart.CFrame = CFrame.new(-3790, 10, -1250)
            end
            task.wait(1)
        end
    end
end

-- Inicia
DoSeaFarm()
