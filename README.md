-- LocalScript em StarterPlayer > StarterPlayerScripts

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Aguarda o personagem carregar
local function getCharacter()
    local character = LocalPlayer.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") then
        character = LocalPlayer.CharacterAdded:Wait()
    end
    return character
end

local Character = getCharacter()
local RootPart = Character:WaitForChild("HumanoidRootPart")

-- Configurações
local ATTACK_RANGE = 20
local ATTACK_INTERVAL = 1 -- segundos

-- Função para encontrar o inimigo mais próximo
local function getClosestEnemy()
    local closest = nil
    local closestDistance = ATTACK_RANGE

    for _, npc in pairs(workspace:GetChildren()) do
        if npc:IsA("Model") and npc:FindFirstChild("Humanoid") and npc:FindFirstChild("HumanoidRootPart") then
            local distance = (npc.HumanoidRootPart.Position - RootPart.Position).Magnitude
            if distance <= closestDistance and npc.Humanoid.Health > 0 then
                closest = npc
                closestDistance = distance
            end
        end
    end

    return closest
end

-- Loop de ataque automático
task.spawn(function()
    while true do
        local target = getClosestEnemy()
        if target then
            print("✅ Inimigo encontrado: " .. target.Name)
            -- Aqui você pode adicionar uma função de ataque, por exemplo:
            -- ataca(target)
        else
            print("❌ Nenhum inimigo por perto.")
        end
        task.wait(ATTACK_INTERVAL)
    end
end)
