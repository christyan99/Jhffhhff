-- Minatox Hub - GUI estilo Minato (Amarelo)
loadstring(game:HttpGet("https://raw.githubusercontent.com/x3fall3n/uilib/main/YellowLib.lua"))()

local Window = Library:CreateWindow("Minatox Hub - Blox Fruits", Color3.fromRGB(255, 204, 0), Enum.KeyCode.RightControl)

-- Abas
local FarmTab = Window:CreateTab("Auto Farm")

-- Configurações Globais
getgenv().Settings = {
    AutoFarmLevel = false,
    AutoAttack = false
}

-- Função de ataque (funciona no mobile / Arceus X)
function AttackNPC(npc)
    local plr = game.Players.LocalPlayer
    local chr = plr.Character
    if not chr then return end
    local tool = chr:FindFirstChildOfClass("Tool")
    if tool and tool:FindFirstChild("Handle") and npc:FindFirstChild("HumanoidRootPart") then
        chr.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0, 10, 0)
        firetouchinterest(tool.Handle, npc.HumanoidRootPart, 0)
        firetouchinterest(tool.Handle, npc.HumanoidRootPart, 1)
    end
end

-- Auto Farm e Auto Attack
spawn(function()
    while task.wait(0.3) do
        if Settings.AutoFarmLevel then
            pcall(function()
                for i, npc in pairs(workspace.Enemies:GetChildren()) do
                    if npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                        for _, tool in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
                            if tool:IsA("Tool") then
                                game.Players.LocalPlayer.Character.Humanoid:EquipTool(tool)
                                break
                            end
                        end
                        if Settings.AutoAttack then
                            AttackNPC(npc)
                        end
                        break
                    end
                end
            end)
        end
    end
end)

-- Botões da GUI
FarmTab:CreateToggle("Auto Farm Level", function(value)
    Settings.AutoFarmLevel = value
end)

FarmTab:CreateToggle("Auto Attack", function(value)
    Settings.AutoAttack = value
end)
