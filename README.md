--=========================
-- BOTÃO PARA ABRIR/FECHAR MENU
--=========================

local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Parent = player:WaitForChild("PlayerGui")
gui.ResetOnSpawn = false

-- Botão ABRIR/FECHAR
local botao = Instance.new("TextButton")
botao.Size = UDim2.new(0, 120, 0, 40)

-- Botão mais para cima
botao.Position = UDim2.new(0.5, -60, 0.4, 0)

botao.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
botao.TextColor3 = Color3.fromRGB(255, 255, 255)
botao.Text = "ABRIR"
botao.Parent = gui

-- Botão arrastável
botao.Active = true
botao.Draggable = true

-- Variáveis
local menu = nil
local aberto = false

-- Função para carregar menu
local function carregarMenu()
	if menu then return end

	local retorno = loadstring(game:HttpGet("https://pastebin.com/raw/MzugyTz9"))()

	if typeof(retorno) == "Instance" then
		menu = retorno
		menu.Parent = gui
		menu.Enabled = false
		if menu:FindFirstChildWhichIsA("Frame") then
			menu.Visible = false
		end
	else
		for _, v in pairs(player.PlayerGui:GetChildren()) do
			if v:IsA("ScreenGui") and v.Name ~= gui.Name then
				menu = v
			end
		end
	end

	--===============================
	-- TROCAR NOME DO MENU + ADICIONAR TIKTOK (POSICIONADO ABAIXO DO NOME)
	--===============================
	if menu then
		for _, obj in pairs(menu:GetDescendants()) do
			
			-- Trocar nome principal
			if (obj:IsA("TextLabel") or obj:IsA("TextButton")) and string.lower(obj.Text) == "pc rblx v1.8" then
				-- muda o texto do título
				obj.Text = "REDZ_MAX07"

				-- cria o label do TikTok posicionado imediatamente abaixo do título
				local tiktok = Instance.new("TextLabel")
				tiktok.Name = "TikTokLabel"
				-- largura igual ao título, altura fixa
				tiktok.Size = UDim2.new(obj.Size.X.Scale, obj.Size.X.Offset, 0, 16)
				-- posiciona embaixo somando a posição Y do título com sua altura (mantém Scale/Offset da X)
				tiktok.Position = UDim2.new(
					obj.Position.X.Scale, obj.Position.X.Offset,
					obj.Position.Y.Scale, obj.Position.Y.Offset + obj.Size.Y.Offset + 4
				)
				-- herdar alguns atributos para ficar alinhado como o título
				tiktok.AnchorPoint = obj.AnchorPoint
				tiktok.TextXAlignment = obj.TextXAlignment
				tiktok.BackgroundTransparency = 1
				tiktok.Text = "TIKTOK: t7maxz"
				tiktok.TextColor3 = Color3.fromRGB(0, 255, 0) -- VERDE
				tiktok.TextScaled = true
				tiktok.Font = obj.Font or Enum.Font.SourceSans
				tiktok.Parent = obj.Parent
			end

		end
	end

	--===============================
	-- REMOVER TEXTOS INDESEJADOS
	--===============================
	if menu then
		for _, obj in pairs(menu:GetDescendants()) do
			if obj:IsA("TextLabel") or obj:IsA("TextButton") then
				
				local t = string.lower(obj.Text)

				if t:find("hide/show gui")
				or t:find("hide/show")
				or t:find("toggle aimbot")
				or t == "k"
				or t == "e" then
					obj.Text = ""
				end
			end
		end
	end
end

-- Atualizar menu (abrir/fechar)
local function atualizarMenu()
	if not menu then return end

	if menu:IsA("ScreenGui") then
		menu.Enabled = aberto
	end

	local frame = menu:FindFirstChildWhichIsA("Frame")
	if frame then
		frame.Visible = aberto
	end
end

-- Clique no botão
botao.MouseButton1Click:Connect(function()
	carregarMenu()

	aberto = not aberto
	atualizarMenu()

	if aberto then
		botao.Text = "FECHAR"
	else
		botao.Text = "ABRIR"
	end
end)
