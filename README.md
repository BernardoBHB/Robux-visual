local player = game.Players.LocalPlayer
local gui = script.Parent
local prompt = gui:WaitForChild("PromptFrame")
local success = gui:WaitForChild("SuccessFrame")
local buyButton = prompt:WaitForChild("BuyButton")

-- Configurações Iniciais
prompt.Visible = true
success.Visible = false

local function fakePurchase()
	-- 1. Efeito visual de "Processando"
	buyButton.Text = "Processando..."
	buyButton.Active = false
	task.wait(1.5) -- Simula o delay da rede
	
	-- 2. Esconde o prompt e mostra o sucesso
	prompt.Visible = false
	success.Visible = true
	
	-- Som de compra (opcional, se você tiver o ID do som)
	local sound = Instance.new("Sound", game.Workspace)
	sound.SoundId = "rbxassetid://6042053626" -- Som clássico de sucesso
	sound:Play()
	
	-- 3. Fecha a mensagem após 3 segundos
	task.wait(3)
	success.Visible = false
end

buyButton.MouseButton1Click:Connect(fakePurchase)
