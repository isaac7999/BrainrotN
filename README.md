--[[ 
✅ UI Completa Mobile - Pulsar X Style
✔️ Speed Boost corrigido (sem travar pulo)
✔️ Campo para definir velocidade
✔️ Vida 0 (imortal)
✔️ Alterar gravidade
✔️ Alterar potência do pulo
✔️ ESP Nick (pequeno)
]]

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "PainelPulsarX"

-- UI Principal
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 230, 0, 330)
frame.Position = UDim2.new(0.02, 0, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
frame.BorderSizePixel = 2
frame.Active = true
frame.Draggable = true

-- Título
local titulo = Instance.new("TextLabel", frame)
titulo.Size = UDim2.new(1, 0, 0, 30)
titulo.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
titulo.Text = "Painel Pulsar X 👣"
titulo.TextColor3 = Color3.new(1, 1, 1)
titulo.TextScaled = true

-- Campo velocidade
local speedInput = Instance.new("TextBox", frame)
speedInput.Size = UDim2.new(1, -20, 0, 30)
speedInput.Position = UDim2.new(0, 10, 0, 35)
speedInput.PlaceholderText = "Velocidade (ex: 80)"
speedInput.Text = ""
speedInput.TextScaled = true
speedInput.TextColor3 = Color3.new(1, 1, 1)
speedInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

-- Botão speed
local speedBtn = Instance.new("TextButton", frame)
speedBtn.Size = UDim2.new(1, -20, 0, 30)
speedBtn.Position = UDim2.new(0, 10, 0, 70)
speedBtn.Text = "Ativar Speed Boost"
speedBtn.TextScaled = true
speedBtn.TextColor3 = Color3.new(1, 1, 1)
speedBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

-- Botão Vida 0
local vidaBtn = Instance.new("TextButton", frame)
vidaBtn.Size = UDim2.new(1, -20, 0, 30)
vidaBtn.Position = UDim2.new(0, 10, 0, 110)
vidaBtn.Text = "Vida 0 (Imortal)"
vidaBtn.TextScaled = true
vidaBtn.TextColor3 = Color3.new(1, 1, 1)
vidaBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

-- Campo gravidade
local gravInput = Instance.new("TextBox", frame)
gravInput.Size = UDim2.new(1, -20, 0, 30)
gravInput.Position = UDim2.new(0, 10, 0, 150)
gravInput.PlaceholderText = "Gravidade (padrão: 196)"
gravInput.Text = ""
gravInput.TextScaled = true
gravInput.TextColor3 = Color3.new(1, 1, 1)
gravInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

-- Botão aplicar gravidade
local gravBtn = Instance.new("TextButton", frame)
gravBtn.Size = UDim2.new(1, -20, 0, 30)
gravBtn.Position = UDim2.new(0, 10, 0, 185)
gravBtn.Text = "Aplicar Gravidade"
gravBtn.TextScaled = true
gravBtn.TextColor3 = Color3.new(1, 1, 1)
gravBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

-- Campo potência de pulo
local jumpInput = Instance.new("TextBox", frame)
jumpInput.Size = UDim2.new(1, -20, 0, 30)
jumpInput.Position = UDim2.new(0, 10, 0, 225)
jumpInput.PlaceholderText = "Potência de Pulo (ex: 50)"
jumpInput.Text = ""
jumpInput.TextScaled = true
jumpInput.TextColor3 = Color3.new(1, 1, 1)
jumpInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

-- Botão aplicar pulo
local jumpBtn = Instance.new("TextButton", frame)
jumpBtn.Size = UDim2.new(1, -20, 0, 30)
jumpBtn.Position = UDim2.new(0, 10, 0, 260)
jumpBtn.Text = "Aplicar Potência de Pulo"
jumpBtn.TextScaled = true
jumpBtn.TextColor3 = Color3.new(1, 1, 1)
jumpBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

-- Botão ESP Nick
local espBtn = Instance.new("TextButton", frame)
espBtn.Size = UDim2.new(1, -20, 0, 30)
espBtn.Position = UDim2.new(0, 10, 0, 295)
espBtn.Text = "Ativar ESP Nick"
espBtn.TextScaled = true
espBtn.TextColor3 = Color3.new(1, 1, 1)
espBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

-- VARIÁVEIS GLOBAIS
local speedAtivo = false
local conexaoSpeed
local espAtivo = false

-- SPEED BOOST SEM ATRAPALHAR PULO
local function toggleSpeed()
	if not speedAtivo then
		local valor = tonumber(speedInput.Text)
		if not valor then
			speedInput.Text = "Número inválido"
			return
		end
		speedAtivo = true
		speedBtn.Text = "Desativar Speed"

		conexaoSpeed = RunService.RenderStepped:Connect(function()
			local char = player.Character
			if char and char:FindFirstChild("HumanoidRootPart") and char:FindFirstChildWhichIsA("Humanoid") then
				local hrp = char.HumanoidRootPart
				local hum = char:FindFirstChildWhichIsA("Humanoid")
				local dir = hum.MoveDirection
				if dir.Magnitude > 0 then
					local vel = hrp.Velocity
					hrp.Velocity = Vector3.new(dir.X * valor, vel.Y, dir.Z * valor)
				end
			end
		end)
	else
		speedAtivo = false
		speedBtn.Text = "Ativar Speed Boost"
		if conexaoSpeed then
			conexaoSpeed:Disconnect()
			conexaoSpeed = nil
		end
	end
end

-- VIDA 0 IMORTAL
local function vidaZero()
	local char = player.Character
	if char and char:FindFirstChild("Humanoid") then
		char.Humanoid.Health = 0.1
		coroutine.wrap(function()
			while wait(0.2) do
				if char and char:FindFirstChild("Humanoid") and char.Humanoid.Health <= 0.1 then
					char.Humanoid.Health = 0.1
				end
			end
		end)()
	end
end

-- ALTERAR GRAVIDADE
local function aplicarGravidade()
	local valor = tonumber(gravInput.Text)
	if valor then
		Workspace.Gravity = valor
	else
		gravInput.Text = "Número inválido"
	end
end

-- ALTERAR POTÊNCIA DE PULO
local function aplicarPulo()
	local valor = tonumber(jumpInput.Text)
	if valor and player.Character and player.Character:FindFirstChildWhichIsA("Humanoid") then
		player.Character:FindFirstChildWhichIsA("Humanoid").UseJumpPower = true
		player.Character:FindFirstChildWhichIsA("Humanoid").JumpPower = valor
	else
		jumpInput.Text = "Número inválido"
	end
end

-- ESP NICK PEQUENO
local function ativarESP()
	if espAtivo then return end
	espAtivo = true
	for _, plr in pairs(Players:GetPlayers()) do
		if plr ~= player then
			local function criarESP()
				local char = plr.Character or plr.CharacterAdded:Wait()
				local head = char:WaitForChild("Head", 5)
				if not head then return end
				local tag = Instance.new("BillboardGui", head)
				tag.Name = "ESPTag"
				tag.Size = UDim2.new(0, 100, 0, 20)
				tag.StudsOffset = Vector3.new(0, 2, 0)
				tag.AlwaysOnTop = true

				local texto = Instance.new("TextLabel", tag)
				texto.Size = UDim2.new(1, 0, 1, 0)
				texto.BackgroundTransparency = 1
				texto.TextColor3 = Color3.new(1, 1, 1)
				texto.TextScaled = true
				texto.TextSize = 10
				texto.Text = plr.Name
				texto.Font = Enum.Font.SourceSans
			end
			coroutine.wrap(criarESP)()
		end
	end
end

-- CONECTAR BOTÕES
speedBtn.MouseButton1Click:Connect(toggleSpeed)
vidaBtn.MouseButton1Click:Connect(vidaZero)
gravBtn.MouseButton1Click:Connect(aplicarGravidade)
jumpBtn.MouseButton1Click:Connect(aplicarPulo)
espBtn.MouseButton1Click:Connect(ativarESP)
