--[[ 
    SIMULADOR DE ROBUX REALISTA (BHB EDITION)
    Funcionalidades: Interface dinâmica, Sons Oficiais, Comando de Chat.
--]]

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer

-- 1. CRIANDO A INTERFACE AUTOMATICAMENTE (Para não dar erro)
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "BHB_RobuxSim"
screenGui.Parent = player:WaitForChild("StarterGui")
screenGui.ResetOnSpawn = false

-- Frame Principal (A Janela)
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 380, 0, 240)
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundColor3 = Color3.fromRGB(35, 37, 39)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = ToolAddress.new(0, 10)
corner.Parent = mainFrame

-- Título do Item
local itemName = Instance.new("TextLabel")
itemName.Size = UDim2.new(1, 0, 0, 50)
itemName.Text = "Limited Edition Item"
itemName.TextColor3 = Color3.fromRGB(255, 255, 255)
itemName.Font = Enum.Font.GothamBold
itemName.TextSize = 18
itemName.BackgroundTransparency = 1
itemName.Parent = mainFrame

-- Preço (Robux)
local priceLabel = Instance.new("TextLabel")
priceLabel.Size = UDim2.new(1, 0, 0, 40)
priceLabel.Position = UDim2.new(0, 0, 0.3, 0)
priceLabel.Text = "R$ 50,000"
priceLabel.TextColor3 = Color3.fromRGB(0, 255, 127)
priceLabel.Font = Enum.Font.GothamBold
priceLabel.TextSize = 24
priceLabel.BackgroundTransparency = 1
priceLabel.Parent = mainFrame

-- Botão de Compra (Verde)
local buyBtn = Instance.new("TextButton")
buyBtn.Name = "BuyButton"
buyBtn.Size = UDim2.new(0, 300, 0, 45)
buyBtn.Position = UDim2.new(0.5, 0, 0.7, 0)
buyBtn.AnchorPoint = Vector2.new(0.5, 0)
buyBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 100)
buyBtn.Text = "Comprar"
buyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
buyBtn.Font = Enum.Font.GothamBold
buyBtn.TextSize = 18
buyBtn.AutoButtonColor = true
buyBtn.Parent = mainFrame

local btnCorner = Instance.new("UICorner")
btnCorner.Parent = buyBtn

-- 2. LÓGICA DE FUNCIONAMENTO
local function showNotification(txt)
    priceLabel.Text = txt
    priceLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
end

buyBtn.MouseButton1Click:Connect(function()
    buyBtn.Active = false
    buyBtn.Text = "Processando..."
    
    -- Som de processamento (clique)
    local s = Instance.new("Sound", game.Workspace)
    s.SoundId = "rbxassetid://12222242"
    s:Play()
    
    task.wait(2.2) -- O tempo exato de verificação do Roblox
    
    -- Som de SUCESSO REAL
    local s2 = Instance.new("Sound", game.Workspace)
    s2.SoundId = "rbxassetid://6042053626"
    s2:Play()
    
    buyBtn.Text = "Concluído!"
    buyBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    showNotification("Compra Realizada!")
    
    task.wait(2)
    mainFrame.Visible = false
    -- Reseta para a próxima vez
    buyBtn.Text = "Comprar"
    buyBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 100)
    buyBtn.Active = true
end)

-- 3. COMANDO /SET NO CHAT
player.Chatted:Connect(function(msg)
    local args = msg:lower():split(" ")
    if args[1] == "/set" and args[2] then
        priceLabel.Text = "R$ " .. args[2]
        priceLabel.TextColor3 = Color3.fromRGB(0, 255, 127)
        mainFrame.Visible = true
        
        -- Efeito de abrir suave
        mainFrame.Size = UDim2.new(0, 0, 0, 0)
        mainFrame:TweenSize(UDim2.new(0, 380, 0, 240), "Out", "Back", 0.4, true)
    end
end)

print("Sistema BHB carregado com sucesso!")
