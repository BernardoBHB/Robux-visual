local player = game.Players.LocalPlayer
local button = script.Parent
local displayLabel = button.Parent:WaitForChild("TextLabel") -- Nome do seu label de saldo

local robuxFake = 0

-- Função para formatar o número (ex: 1,000)
local function formatNumber(n)
	return tostring(n):reverse():gsub("%d%d%d", "%1,"):reverse():gsub("^,", "")
end

button.MouseButton1Click:Connect(function()
	-- Define quanto ganha por clique
	local ganho = 500 
	robuxFake = robuxFake + ganho
	
	-- Atualiza o texto na tela
	displayLabel.Text = "R$ " .. formatNumber(robuxFake)
	
	-- Efeito visual simples de escala ao clicar
	button:TweenSize(UDim2.new(0, 190, 0, 45), "Out", "Quad", 0.1, true)
	task.wait(0.1)
	button:TweenSize(UDim2.new(0, 200, 0, 50), "Out", "Quad", 0.1, true)
end)
