local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local ContentProvider = game:GetService("ContentProvider")
local Lighting = game:GetService("Lighting")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ID DA IMAGEM DE ANALOG HORROR
local ID_IMAGEM = "rbxassetid://12433698744"

-- 1. FUNÇÃO DO EFEITO PRETO E BRANCO (CONTRASTE NEGATIVO)
local function aplicarEfeitoNegativo()
	-- Limpa efeitos antigos para não acumular
	for _, v in pairs(Lighting:GetChildren()) do
		if v:IsA("ColorCorrectionEffect") then
			v:Destroy()
		end
	end

	-- Criar novo efeito
	local color = Instance.new("ColorCorrectionEffect")
	color.Name = "BW_PERMANENTE"
	color.Saturation = -1
	color.Contrast = -10
	color.Brightness = 0.1
	color.Parent = Lighting
end

-- INICIAR O EFEITO LOGO NO COMEÇO
aplicarEfeitoNegativo()

-- 2. INTERFACE DA IMAGEM (FUNDO)
local guiImagem = Instance.new("ScreenGui")
guiImagem.Name = "Efeito_Terror_Fundo"
guiImagem.IgnoreGuiInset = true
guiImagem.DisplayOrder = 5
guiImagem.Parent = playerGui

local imagemLabel = Instance.new("ImageLabel")
imagemLabel.Size = UDim2.new(1, 0, 1, 0)
imagemLabel.BackgroundTransparency = 1
imagemLabel.Image = ID_IMAGEM
imagemLabel.ImageTransparency = 1 -- Começa invisível
imagemLabel.ScaleType = Enum.ScaleType.Crop
imagemLabel.Parent = guiImagem

-- 3. INTERFACE DE DIÁLOGO
local guiDialogo = Instance.new("ScreenGui")
guiDialogo.Name = "Narrativa_Final_Unificada"
guiDialogo.DisplayOrder = 10
guiDialogo.Parent = playerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0.7, 0, 0.22, 0)
frame.Position = UDim2.new(0.15, 0, 0.73, 0)
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.BackgroundTransparency = 0.3
frame.BorderSizePixel = 2
frame.BorderColor3 = Color3.new(1, 1, 1)
frame.Parent = guiDialogo

local texto = Instance.new("TextLabel")
texto.Size = UDim2.new(0.9, 0, 0.8, 0)
texto.Position = UDim2.new(0.05, 0, 0.1, 0)
texto.BackgroundTransparency = 1
texto.TextColor3 = Color3.new(1, 1, 1)
texto.TextScaled = true
texto.RichText = true 
texto.Font = Enum.Font.Antique
texto.Text = ""
texto.Parent = frame

-- FUNÇÕES DE TEXTO
local function formatar(f)
	f = f:gsub("ANJOS", "<font color='rgb(255,255,0)'>ANJOS</font>")
	f = f:gsub("DEMÔNIOS", "<font color='rgb(255,0,0)'>DEMÔNIOS</font>")
	return f
end

local function narrar(msg)
	texto.Text = formatar(msg)
	texto.MaxVisibleGraphemes = 0
	local tamanho = utf8.len(msg)
	for i = 1, tamanho do
		texto.MaxVisibleGraphemes = i
		task.wait(0.05)
	end
	task.wait(3.5)
end

-- 4. INÍCIO DA SEQUÊNCIA
ContentProvider:PreloadAsync({imagemLabel}) 
task.wait(2)

-- Narração (Já em Preto e Branco)
narrar("Aproxime-se... Eu costumava contar essa história para ninar as crianças.")
narrar("Dizia que os ANJOS voavam pelo céu, protegendo nosso sono... mas o pesadelo agora é real.")
narrar("deus morreu. O trono está vazio e o silêncio é a nossa única herança.")
narrar("Os ANJOS perderam a fé. Perderam toda a esperança.")
narrar("Os DEMÔNIOS finalmente descobriram a verdade... e agora eles que mandam.")
narrar("Não adianta se esconder, o que resta é esperar o fim.")
narrar("Os DEMÔNIOS conseguem sentir o cheiro da alma... eles conseguem sentir o cheiro do seu medo.")
narrar("Eles sabem, sempre vão saber... porque agora o destino pertence a eles.")

-- 5. APARECIMENTO DA IMAGEM FINAL (FADE-IN)
local tempoFade = 8
local passos = 100
for i = 1, passos do
	imagemLabel.ImageTransparency = 1 - (i / passos)
	task.wait(tempoFade / passos)
end

task.wait(2) 

-- 6. KICK FINAL
local evento = ReplicatedStorage:FindFirstChild("KickPlayerEvent")
if evento then
	evento:FireServer("Eles sentiram o seu medo. O fim chegou.")
else
	player:Kick("O destino foi selado. O inferno ganhou.")
end
