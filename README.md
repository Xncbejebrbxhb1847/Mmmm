-- Configurações Gerais
local Delay = 1
local StardFarm = false
local FarmMag = false

-- Funções Auxiliares
local function AutoHaki()
    -- Implementar ativação do Haki automático, se necessário
end

local function EquipWeapon(weaponName)
    for _, tool in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
        if tool.Name == weaponName then
            game.Players.LocalPlayer.Character.Humanoid:EquipTool(tool)
        end
    end
end

local function topos(position)
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = position
end

-- Automação de Farm
spawn(function()
    while task.wait() do
        pcall(function()
            if StardFarm then
                for _, enemy in pairs(game.Workspace.Enemies:GetChildren()) do
                    if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
                        repeat
                            AutoHaki()
                            EquipWeapon(_G.SelectWeapon or "DefaultWeapon")
                            topos(enemy.HumanoidRootPart.CFrame)
                            game:GetService("VirtualUser"):CaptureController()
                            game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672))
                            task.wait(Delay)
                        until not StardFarm or enemy.Humanoid.Health <= 0
                    end
                end
            end
        end)
    end
end)

-- Transformação de Raça
spawn(function()
    while task.wait() do
        pcall(function()
            if _G.AutoRace then
                local player = game.Players.LocalPlayer
                if player.Character.RaceTransformed.Value then
                    StardFarm = false
                    topos(CFrame.new(216.211, 126.935, -12599.073)) -- Posição inicial
                else
                    StardFarm = true
                end
            end
        end)
    end
end)

-- Automação de Quest
spawn(function()
    while task.wait() do
        pcall(function()
            if _G.AutoQuestRace then
                for _, enemy in pairs(game.Workspace.Enemies:GetDescendants()) do
                    if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
                        repeat
                            enemy.Humanoid.Health = 0
                            task.wait(0.1)
                        until enemy.Humanoid.Health <= 0
                    end
                end
            end
        end)
    end
end)

-- Interação com Teclas
spawn(function()
    while task.wait() do
        pcall(function()
            if _G.AutoRace then
                game:GetService("VirtualInputManager"):SendKeyEvent(true, "Y", false, game)
                task.wait(0.1)
                game:GetService("VirtualInputManager"):SendKeyEvent(false, "Y", false, game)
            end
        end)
    end
end)
