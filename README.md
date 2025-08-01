-- Sistema de Key + Painel Chiquitazzqx Hub

local correctKey = "chiquitazzqxkey1345"

local function createKeyUI()
    local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
    local Frame = Instance.new("Frame", ScreenGui)
    local UICorner = Instance.new("UICorner", Frame)
    local TextBox = Instance.new("TextBox", Frame)
    local CheckButton = Instance.new("TextButton", Frame)
    local GetKeyButton = Instance.new("TextButton", Frame)
    local InfoLabel = Instance.new("TextLabel", Frame)

    ScreenGui.Name = "KeySystem"
    Frame.Name = "Main"
    Frame.Size = UDim2.new(0, 300, 0, 180)
    Frame.Position = UDim2.new(0.5, -150, 0.5, -90)
    Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    Frame.BorderSizePixel = 0

    TextBox.PlaceholderText = "Insira sua Key aqui"
    TextBox.Size = UDim2.new(0, 260, 0, 30)
    TextBox.Position = UDim2.new(0, 20, 0, 20)
    TextBox.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    TextBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    TextBox.ClearTextOnFocus = false

    CheckButton.Text = "Verificar Key"
    CheckButton.Size = UDim2.new(0, 120, 0, 30)
    CheckButton.Position = UDim2.new(0, 20, 0, 60)
    CheckButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    CheckButton.TextColor3 = Color3.fromRGB(255, 255, 255)

    GetKeyButton.Text = "Pegar Key"
    GetKeyButton.Size = UDim2.new(0, 120, 0, 30)
    GetKeyButton.Position = UDim2.new(0, 160, 0, 60)
    GetKeyButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    GetKeyButton.TextColor3 = Color3.fromRGB(255, 255, 255)

    InfoLabel.Text = "A key está no meu TTK. Copie: chiquitazzqxkey1345"
    InfoLabel.Size = UDim2.new(1, -20, 0, 60)
    InfoLabel.Position = UDim2.new(0, 10, 0, 100)
    InfoLabel.TextWrapped = true
    InfoLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    InfoLabel.BackgroundTransparency = 1

    CheckButton.MouseButton1Click:Connect(function()
        if TextBox.Text == correctKey then
            ScreenGui:Destroy()

            -- PAINEL CHIQUITAZZQX HUB COMPLETO AQUI ABAIXO
            loadstring(game:HttpGet("https://raw.githubusercontent.com/chiquitazzqx/Chiquitazzqx12/main/finalrgbpainel.lua"))()
        
        else
            InfoLabel.Text = "❌ Key incorreta! Verifique no TTK."
        end
    end)

    GetKeyButton.MouseButton1Click:Connect(function()
        setclipboard(correctKey)
        InfoLabel.Text = "✅ Key copiada! Vá até o TTK e cole."
    end)
end

createKeyUI()
