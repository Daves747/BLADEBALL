-- LocalScript (coloque em StarterPlayerScripts ou StarterCharacterScripts)

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local userInputService = game:GetService("UserInputService")
local debounce = false -- Evitar múltiplos acionamentos seguidos

-- Função para rebater a bola
local function RebaterBola(bola)
    -- Verifica se a bola é próxima o suficiente para ser rebatida
    local distancia = (humanoidRootPart.Position - bola.Position).Magnitude
    if distancia <= 10 then  -- Distância de ativação, ajuste conforme necessário
        -- Aplica uma força na bola para rebatê-la
        local direction = (bola.Position - humanoidRootPart.Position).unit
        local força = 500  -- Ajuste o valor da força conforme necessário
        bola.Velocity = direction * força  -- Aplica a velocidade para rebater a bola
    end
end

-- Detectar quando a tecla 0 é pressionada
userInputService.InputBegan:Connect(function(input, isProcessed)
    if isProcessed then return end  -- Ignora se o input já for processado (por exemplo, em menus)
    
    if input.KeyCode == Enum.KeyCode.Zero and not debounce then
        debounce = true  -- Impede múltiplos acionamentos em sequência rápida
        -- Procura a bola no jogo
        local bola = game.Workspace:FindFirstChild("Bola")  -- Aqui, a bola deve ter o nome "Bola" no Workspace
        
        if bola then
            -- Chama a função para rebater a bola
            RebaterBola(bola)
        end
        
        -- Reseta o debounce depois de um tempo (evitar múltiplos cliques rápidos)
        wait(0.5)
        debounce = false
    end
end)
