--[[ 
    =========================================================
    BHB PLATAFORM - SISTEMA DE COMPRA FAKE V4.0
    =========================================================
    Instalação: Coloque este LocalScript em StarterPlayerScripts.
    Uso no Mapa: Crie uma Part, coloque um ClickDetector e um 
    IntValue chamado "FakeItemID" com a ID do item oficial.
    =========================================================
]]--

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local MarketplaceService = game:GetService("MarketplaceService")
local Lighting = game:GetService("Lighting")

local player = Players.LocalPlayer
local fakeBalance = 0

-- ==========================================
-- 1. CONSTRUÇÃO DO BHB HUB (PAINEL LATERAL)
-- ==========================================
local coreGui = Instance.new("ScreenGui")
coreGui.Name = "BHB_System_UI"
coreGui.ResetOnSpawn = false
coreGui.Parent = player:WaitForChild("PlayerGui")

local hubFrame = Instance.new("Frame")
hubFrame.Size = UDim2.new(0, 200, 0, 100)
hubFrame.Position = UDim2.new(0, 20, 0.5, 0)
hubFrame.AnchorPoint = Vector2.new(0, 0.5)
hubFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
hubFrame.BorderSizePixel = 0
hubFrame.Parent = coreGui

local hubCorner = Instance.new("UICorner")
hubCorner.CornerRadius = UDim.new(0, 8)
hubCorner.Parent = hubFrame

local hubTitle = Instance.new("TextLabel")
hubTitle.Size = UDim2.new(1, 0, 0, 30)
hubTitle.BackgroundTransparency = 1
hubTitle.Text = "BHB HUB - Saldo"
hubTitle.TextColor3 = Color3.fromRGB(0, 255, 127)
hubTitle.Font = Enum.Font.GothamBold
hubTitle.TextSize = 14
hubTitle.Parent = hubFrame

local balanceInput = Instance.new("TextBox")
balanceInput.Size = UDim2.new(0.8, 0, 0, 35)
balanceInput.Position = UDim2.new(0.1, 0, 0.4, 0)
balanceInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
balanceInput.TextColor3 = Color3.fromRGB(255, 255, 255)
balanceInput.Font = Enum.Font.Gotham
balanceInput.TextSize = 16
balanceInput.PlaceholderText = "Digite o saldo..."
balanceInput.Text = "0"
balanceInput.Parent = hubFrame

local inputCorner = Instance.new("UICorner")
inputCorner.CornerRadius = UDim.new(0, 6)
inputCorner.Parent = balanceInput

-- Atualiza saldo pelo Hub
balanceInput.FocusLost:Connect(function()
	local num = tonumber(balanceInput.Text)
	if num then
		fakeBalance = num
		balanceInput.Text = "R$ " .. fakeBalance
	else
		balanceInput.Text = "R$ " .. fakeBalance
	end
end)

-- ==========================================
-- 2. CONSTRUÇÃO DA TELA DE COMPRA (PROMPT)
-- ==========================================
local blurEffect = Instance.new("BlurEffect")
blurEffect.Size = 0
blurEffect.Parent = Lighting

local promptContainer = Instance.new("Frame")
promptContainer.Size = UDim2.new(1, 0, 1, 0)
promptContainer.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
promptContainer.BackgroundTransparency = 1
promptContainer.Active = true
promptContainer.Visible = false
promptContainer.Parent = coreGui

local mainPrompt = Instance.new("Frame")
mainPrompt.Size = UDim2.new(0, 340, 0, 360)
mainPrompt.Position = UDim2.new(0.5, 0, 0.5, 0)
mainPrompt.AnchorPoint = Vector2.new(0.5, 0.5)
mainPrompt.BackgroundColor3 = Color3.fromRGB(35, 37, 39)
mainPrompt.BackgroundTransparency = 1
mainPrompt.Parent = promptContainer

local promptCorner = Instance.new("UICorner")
promptCorner.CornerRadius = UDim.new(0, 12)
promptCorner.Parent = mainPrompt

local itemIcon = Instance.new("ImageLabel")
itemIcon.Size = UDim2.new(0, 120, 0, 120)
itemIcon.Position = UDim2.new(0.5, 0, 0.05, 0)
itemIcon.AnchorPoint = Vector2.new(0.5, 0)
itemIcon.BackgroundTransparency = 1
itemIcon.Parent = mainPrompt

local itemName = Instance.new("TextLabel")
itemName.Size = UDim2.new(0.9, 0, 0, 30)
itemName.Position = UDim2.new(0.5, 0, 0.42, 0)
itemName.AnchorPoint = Vector2.new(0.5, 0)
itemName.BackgroundTransparency = 1
itemName.Font = Enum.Font.GothamBold
itemName.TextSize = 20
itemName.TextColor3 = Color3.fromRGB(255, 255, 255)
itemName.TextWrapped = true
itemName.Parent = mainPrompt

local priceText = Instance.new("TextLabel")
priceText.Size = UDim2.new(1, 0, 0, 25)
priceText.Position = UDim2.new(0.5, 0, 0.55, 0)
priceText.AnchorPoint = Vector2.new(0.5, 0)
priceText.BackgroundTransparency = 1
priceText.Font = Enum.Font.GothamMedium
priceText.TextSize = 22
priceText.TextColor3 = Color3.fromRGB(255, 255, 255)
priceText.Parent = mainPrompt

local buyBtn = Instance.new("TextButton")
buyBtn.Size = UDim2.new(0.85, 0, 0, 45)
buyBtn.Position = UDim2.new(0.5, 0, 0.75, 0)
buyBtn.AnchorPoint = Vector2.new(0.5, 0)
buyBtn.BackgroundColor3 = Color3.fromRGB(0, 176, 111)
buyBtn.Font = Enum.Font.GothamBold
buyBtn.TextSize = 18
buyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
buyBtn.Text = "Comprar Item"
buyBtn.Parent = mainPrompt

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 8)
btnCorner.Parent = buyBtn

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(0.95, 0, 0.05, 0)
closeBtn.AnchorPoint = Vector2.new(1, 0)
closeBtn.BackgroundTransparency = 1
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 20
closeBtn.TextColor3 = Color3.fromRGB(150, 150, 150)
closeBtn.Text = "X"
closeBtn.Parent = mainPrompt

-- ==========================================
-- 3. ANIMAÇÕES E LÓGICA DE COMPRA
-- ==========================================
local function togglePrompt(state)
	local info = TweenInfo.new(0.3, Enum.EasingStyle.Quint, Enum.EasingDirection.Out)
	if state then
		promptContainer.Visible = true
		TweenService:Create(promptContainer, info, {BackgroundTransparency = 0.5}):Play()
		TweenService:Create(mainPrompt, info, {BackgroundTransparency = 0}):Play()
		TweenService:Create(blurEffect, info, {Size = 15}):Play()
	else
		local t1 = TweenService:Create(promptContainer, info, {BackgroundTransparency = 1})
		local t2 = TweenService:Create(mainPrompt, info, {BackgroundTransparency = 1})
		TweenService:Create(blurEffect, info, {Size = 0}):Play()
		t1:Play() t2:Play()
		t2.Completed:Wait()
		promptContainer.Visible = false
	end
end

closeBtn.MouseButton1Click:Connect(function() togglePrompt(false) end)

local currentPrice = 0
local isProcessing = false

buyBtn.MouseButton1Click:Connect(function()
	if isProcessing then return end
	isProcessing = true
	
	buyBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
	buyBtn.Text = "Processando..."
	
	local snd = Instance.new("Sound", workspace)
	snd.SoundId = "rbxassetid://12222242"
	snd:Play()
	
	task.wait(2.5) -- Delay realista
	
	if fakeBalance >= currentPrice then
		fakeBalance = fakeBalance - currentPrice
		balanceInput.Text = "R$ " .. fakeBalance 
		
		snd.SoundId = "rbxassetid://6042053626"
		snd:Play()
		
		buyBtn.BackgroundColor3 = Color3.fromRGB(0, 176, 111)
		buyBtn.Text = "Concluído!"
	else
		snd.SoundId = "rbxassetid://12222242"
		snd:Play()
		buyBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
		buyBtn.Text = "Saldo Insuficiente"
	end
	
	task.wait(2)
	togglePrompt(false)
	isProcessing = false
end)

-- ==========================================
-- 4. DETECÇÃO NO MAPA (O GATILHO)
-- ==========================================
local function setupFakeItem(part)
	local clickDetector = part:FindFirstChildOfClass("ClickDetector")
	local itemIDValue = part:FindFirstChild("FakeItemID")
	
	if clickDetector and itemIDValue then
		clickDetector.MouseClick:Connect(function()
			local id = itemIDValue.Value
			
			-- Busca os dados oficiais do Roblox
			local success, info = pcall(function()
				return MarketplaceService:GetProductInfo(id, Enum.InfoType.Asset)
			end)
			
			if success and info then
				currentPrice = info.PriceInRobux or 0
				itemName.Text = info.Name
				priceText.Text = currentPrice > 0 and "R$ " .. currentPrice or "Grátis"
				itemIcon.Image = "rbxthumb://type=Asset&id=" .. id .. "&w=150&h=150"
				
				buyBtn.Text = "Comprar Item"
				buyBtn.BackgroundColor3 = Color3.fromRGB(0, 176, 111)
				togglePrompt(true)
			else
				warn("BHB System: Falha ao carregar a ID do item " .. id)
			end
		end)
	end
end

-- Escaneia o mapa ao iniciar e quando novos itens são adicionados
for _, obj in pairs(workspace:GetDescendants()) do
	if obj:IsA("BasePart") then setupFakeItem(obj) end
end

workspace.DescendantAdded:Connect(function(obj)
	if obj:IsA("BasePart") then
		task.wait(0.1)
		setupFakeItem(obj)
	end
end)

print("Sistema BHB Plataform V4 carregado com sucesso!")
