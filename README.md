--=========================================================
-- CONFIGURAÇÃO
--=========================================================
local CORRECT_KEY = "2011" -- KEY PARA LIBERAR MENU
local TweenService = game:GetService("TweenService")

local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Parent = player:WaitForChild("PlayerGui")
gui.ResetOnSpawn = false

--=========================================================
-- GUI DA KEY (PRETO TOTAL E BONITO)
--=========================================================
local keyFrame = Instance.new("Frame")
keyFrame.Size = UDim2.new(0, 340, 0, 210)
keyFrame.Position = UDim2.new(0.5, -170, 0.5, -105)
keyFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- PRETO TOTAL
keyFrame.BorderSizePixel = 0
keyFrame.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = keyFrame

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 45)
title.Position = UDim2.new(0, 0, 0, 10)
title.BackgroundTransparency = 1
title.Text = "🔑 DIGITE A KEY"
title.TextScaled = true
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Parent = keyFrame

local box = Instance.new("TextBox")
box.Size = UDim2.new(0.8, 0, 0, 45)
box.Position = UDim2.new(0.1, 0, 0.45, 0)
box.BackgroundColor3 = Color3.fromRGB(15, 15, 15) -- PRETO SUAVE
box.TextColor3 = Color3.fromRGB(0, 255, 0)
box.PlaceholderText = "Digite a key..."
box.PlaceholderColor3 = Color3.fromRGB(180, 180, 180)
box.Text = ""
box.Font = Enum.Font.Gotham
box.TextScaled = true
box.Parent = keyFrame

local boxCorner = Instance.new("UICorner")
boxCorner.CornerRadius = UDim.new(0, 8)
boxCorner.Parent = box

local confirmar = Instance.new("TextButton")
confirmar.Size = UDim2.new(0.6, 0, 0, 45)
confirmar.Position = UDim2.new(0.2, 0, 0.75, 0)
confirmar.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- PRETO TOTAL
confirmar.TextColor3 = Color3.fromRGB(255, 255, 255)
confirmar.Text = "CONFIRMAR"
confirmar.TextScaled = true
confirmar.Font = Enum.Font.GothamBold
confirmar.Parent = keyFrame

local confCorner = Instance.new("UICorner")
confCorner.CornerRadius = UDim.new(0, 8)
confCorner.Parent = confirmar

--=========================================================
-- BOTÃO ABRIR/FECHAR (SÓ APARECE APÓS KEY CORRETA)
--=========================================================
local botao = Instance.new("TextButton")
botao.Size = UDim2.new(0, 120, 0, 40)
botao.Position = UDim2.new(0.5, -60, 0.4, 0)
botao.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
botao.TextColor3 = Color3.fromRGB(255, 255, 255)
botao.Text = "ABRIR"
botao.Visible = false
botao.Parent = gui
botao.Active = true
botao.Draggable = true

-- Variáveis do menu
local menu = nil
local aberto = false

--=========================================================
-- FUNÇÃO CARREGAR MENU
--=========================================================
local function carregarMenu()
	if menu then return end

	local retorno = loadstring(game:HttpGet("https://pastebin.com/raw/MzugyTz9"))()

	if typeof(retorno) == "Instance" then
		menu = retorno
		menu.Parent = gui
		menu.Enabled = false
		local frame = menu:FindFirstChildWhichIsA("Frame")
		if frame then frame.Visible = false end
	else
		for _, v in pairs(player.PlayerGui:GetChildren()) do
			if v:IsA("ScreenGui") and v.Name ~= gui.Name then
				menu = v
			end
		end
	end

	-- Alterar Nome + TikTok
	if menu then
		for _, obj in pairs(menu:GetDescendants()) do
			if (obj:IsA("TextLabel") or obj:IsA("TextButton")) 
				and string.lower(obj.Text) == "pc rblx v1.8" then

				obj.Text = "REDZ_MAX07"

				local tiktok = Instance.new("TextLabel")
				tiktok.Name = "TikTokLabel"
				tiktok.Size = UDim2.new(obj.Size.X.Scale, obj.Size.X.Offset, 0, 16)
				tiktok.Position = UDim2.new(
					obj.Position.X.Scale, obj.Position.X.Offset,
					obj.Position.Y.Scale, obj.Position.Y.Offset + obj.Size.Y.Offset + 4
				)
				tiktok.AnchorPoint = obj.AnchorPoint
				tiktok.TextXAlignment = obj.TextXAlignment
				tiktok.BackgroundTransparency = 1
				tiktok.Text = "TIKTOK: t7maxz"
				tiktok.TextColor3 = Color3.fromRGB(0, 255, 0)
				tiktok.TextScaled = true
				tiktok.Font = obj.Font
				tiktok.Parent = obj.Parent
			end
		end
	end

	-- Remover textos indesejados
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

--=========================================================
-- ATUALIZAR MENU
--=========================================================
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

--=========================================================
-- CLIQUE DO BOTÃO ABRIR/FECHAR
--=========================================================
botao.MouseButton1Click:Connect(function()
	carregarMenu()

	aberto = not aberto
	atualizarMenu()

	botao.Text = aberto and "FECHAR" or "ABRIR"
end)

--=========================================================
-- SISTEMA DA KEY
--=========================================================
confirmar.MouseButton1Click:Connect(function()
	if box.Text == CORRECT_KEY then
		keyFrame:Destroy()
		botao.Visible = true
	else
		box.Text = ""
		box.PlaceholderText = "KEY ERRADA!"
		box.PlaceholderColor3 = Color3.fromRGB(255, 0, 0)
	end
end)
