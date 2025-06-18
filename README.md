local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Interface principal
local gui = Instance.new("ScreenGui")
gui.Name = "ContadorGUI"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- Botão de iniciar contagem
local button = Instance.new("TextButton")
button.Text = "Iniciar Contagem"
button.Size = UDim2.new(0, 200, 0, 50)
button.Position = UDim2.new(0.5, -100, 0.9, 0)
button.BackgroundColor3 = Color3.new(0.2, 0.6, 1)
button.TextColor3 = Color3.new(1,1,1)
button.TextScaled = true
button.Font = Enum.Font.SourceSansBold
button.Parent = gui

-- Mensagem final (invisível até o final)
local fimLabel = Instance.new("TextLabel")
fimLabel.Size = UDim2.new(0, 300, 0, 60)
fimLabel.Position = UDim2.new(0.5, -150, 0.8, -70)
fimLabel.BackgroundTransparency = 1
fimLabel.TextScaled = true
fimLabel.TextColor3 = Color3.new(0, 1, 0)
fimLabel.Font = Enum.Font.SourceSansBold
fimLabel.Text = ""
fimLabel.Parent = gui

-- Lista com os números por extenso
local numerosPorExtenso = {
    "UM!", "DOIS!", "TRÊS!", "QUATRO!", "CINCO!", "SEIS!", "SETE!", "OITO!", "NOVE!", "DEZ!",
    "ONZE!", "DOZE!", "TREZE!", "CATORZE!", "QUINZE!", "DEZESSEIS!", "DEZESSETE!", "DEZOITO!", "DEZENOVE!", "VINTE!",
    "VINTE E UM!", "VINTE E DOIS!", "VINTE E TRÊS!", "VINTE E QUATRO!", "VINTE E CINCO!",
    "VINTE E SEIS!", "VINTE E SETE!", "VINTE E OITO!", "VINTE E NOVE!", "TRINTA!",
    "TRINTA E UM!", "TRINTA E DOIS!", "TRINTA E TRÊS!", "TRINTA E QUATRO!", "TRINTA E CINCO!",
    "TRINTA E SEIS!", "TRINTA E SETE!", "TRINTA E OITO!", "TRINTA E NOVE!", "QUARENTA!",
    "QUARENTA E UM!", "QUARENTA E DOIS!", "QUARENTA E TRÊS!", "QUARENTA E QUATRO!", "QUARENTA E CINCO!",
    "QUARENTA E SEIS!", "QUARENTA E SETE!", "QUARENTA E OITO!", "QUARENTA E NOVE!", "CINQUENTA!"
}

-- Evento do chat
local ChatEvent = ReplicatedStorage:WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("SayMessageRequest")

-- Função de contagem
local function enviarContagem()
    button.Visible = false
    fimLabel.Text = ""
    
    for _, numero in ipairs(numerosPorExtenso) do
        ChatEvent:FireServer(numero, "All")
        wait(3.5) -- 3.5 segundos entre cada número
    end

    -- Mensagem final
    fimLabel.Text = "Contagem finalizada"
    wait(5)
    fimLabel.Text = ""
    button.Visible = true
end

-- Quando clicar no botão, iniciar contagem
button.MouseButton1Click:Connect(enviarContagem)
