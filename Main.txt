local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Vehicles = workspace:WaitForChild("Vehicles")

-- Função para pegar o carro do jogador (busca carro cujo nome contém o nome do jogador)
local function GetPlayerCar()
    for _, car in pairs(Vehicles:GetChildren()) do
        if string.find(car.Name:lower(), LocalPlayer.Name:lower()) then
            return car
        end
    end
    return nil
end

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "CarBoostGUI"
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

local Main = Instance.new("Frame")
Main.Size = UDim2.new(0, 280, 0, 160)
Main.Position = UDim2.new(0.5, -140, 0.5, -80)
Main.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true
Main.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Parent = Main
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
Title.Text = "turbine seu veiculo aqui"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18
Title.BorderSizePixel = 0

-- Label e input para velocidade
local SpeedLabel = Instance.new("TextLabel")
SpeedLabel.Parent = Main
SpeedLabel.Size = UDim2.new(0, 140, 0, 20)
SpeedLabel.Position = UDim2.new(0, 10, 0, 45)
SpeedLabel.Text = "Velocidade (TopSpeed):"
SpeedLabel.TextColor3 = Color3.new(1,1,1)
SpeedLabel.BackgroundTransparency = 1
SpeedLabel.Font = Enum.Font.Gotham
SpeedLabel.TextSize = 14

local SpeedInput = Instance.new("TextBox")
SpeedInput.Parent = Main
SpeedInput.Size = UDim2.new(0, 120, 0, 25)
SpeedInput.Position = UDim2.new(0, 150, 0, 43)
SpeedInput.PlaceholderText = "Ex: 1000"
SpeedInput.TextColor3 = Color3.new(1,1,1)
SpeedInput.BackgroundColor3 = Color3.fromRGB(50,50,50)
SpeedInput.ClearTextOnFocus = false
SpeedInput.Font = Enum.Font.Gotham
SpeedInput.TextSize = 14
SpeedInput.Text = "1000"

-- Label e input para turbo
local TurboLabel = Instance.new("TextLabel")
TurboLabel.Parent = Main
TurboLabel.Size = UDim2.new(0, 140, 0, 20)
TurboLabel.Position = UDim2.new(0, 10, 0, 80)
TurboLabel.Text = "Turbo:"
TurboLabel.TextColor3 = Color3.new(1,1,1)
TurboLabel.BackgroundTransparency = 1
TurboLabel.Font = Enum.Font.Gotham
TurboLabel.TextSize = 14

local TurboInput = Instance.new("TextBox")
TurboInput.Parent = Main
TurboInput.Size = UDim2.new(0, 120, 0, 25)
TurboInput.Position = UDim2.new(0, 150, 0, 78)
TurboInput.PlaceholderText = "Ex: 500"
TurboInput.TextColor3 = Color3.new(1,1,1)
TurboInput.BackgroundColor3 = Color3.fromRGB(50,50,50)
TurboInput.ClearTextOnFocus = false
TurboInput.Font = Enum.Font.Gotham
TurboInput.TextSize = 14
TurboInput.Text = "500"

-- Botão aplicar
local ApplyButton = Instance.new("TextButton")
ApplyButton.Parent = Main
ApplyButton.Size = UDim2.new(0, 260, 0, 30)
ApplyButton.Position = UDim2.new(0, 10, 0, 115)
ApplyButton.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
ApplyButton.Text = "Aplicar"
ApplyButton.TextColor3 = Color3.fromRGB(255,255,255)
ApplyButton.Font = Enum.Font.GothamBold
ApplyButton.TextSize = 18
ApplyButton.BorderSizePixel = 0

local function ApplyBoost()
    local car = GetPlayerCar()
    if not car then
        warn("🚫 Carro do jogador não encontrado.")
        return
    end

    local seat = car:FindFirstChild("Body") and car.Body:FindFirstChild("VehicleSeat")
    if not seat then
        warn("🚫 VehicleSeat não encontrado no carro.")
        return
    end

    local speedValue = tonumber(SpeedInput.Text)
    local turboValue = tonumber(TurboInput.Text)
    if not speedValue or not turboValue then
        warn("Digite valores válidos para velocidade e turbo.")
        return
    end

    if seat:FindFirstChild("TopSpeed") then
        seat.TopSpeed.Value = speedValue
    else
        warn("TopSpeed não encontrado no VehicleSeat.")
    end

    if seat:FindFirstChild("Turbo") then
        seat.Turbo.Value = turboValue
    else
        warn("Turbo não encontrado no VehicleSeat.")
    end

    game.StarterGui:SetCore("SendNotification", {
        Title = "✅ Boost Aplicado!",
        Text = "Velocidade e turbo ajustados.",
        Duration = 4
    })
end

ApplyButton.MouseButton1Click:Connect(ApplyBoost)
