-- Configurações do ESP
local ESPColor = Color3.new(1, 1, 1) -- Branco
local BoxThickness = 2 -- Espessura das bordas
local ESPEnabled = true -- Ativar/Desativar ESP

-- Função para criar a box ao redor de um jogador
local function createESP(player)
    -- Verificar se o player possui um modelo
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidPart = player.Character.HumanoidRootPart

        -- Criar um quadro para o ESP
        local box = Drawing.new("Square")
        box.Visible = false
        box.Color = ESPColor
        box.Thickness = BoxThickness
        box.Transparency = 1

        -- Atualizar a posição e visibilidade do box
        game:GetService("RunService").RenderStepped:Connect(function()
            if ESPEnabled and humanoidPart and humanoidPart.Parent then
                local screenPosition, onScreen = workspace.CurrentCamera:WorldToViewportPoint(humanoidPart.Position)

                if onScreen then
                    -- Configurar o tamanho e posição do box
                    local size = Vector2.new(100, 150) -- Tamanho ajustável
                    box.Size = size
                    box.Position = Vector2.new(screenPosition.X - size.X / 2, screenPosition.Y - size.Y / 2)
                    box.Visible = true
                else
                    box.Visible = false
                end
            else
                box.Visible = false
            end
        end)
    end
end

-- Aplicar ESP para todos os jogadores no jogo
for _, player in pairs(game.Players:GetPlayers()) do
    if player ~= game.Players.LocalPlayer then
        createESP(player)
    end
end

-- Detectar novos jogadores entrando no jogo
game.Players.PlayerAdded:Connect(function(player)
    if player ~= game.Players.LocalPlayer then
        player.CharacterAdded:Connect(function()
            createESP(player)
        end)
    end
end)
