-- Chiquitazzqx Mod Menu v2.9 com sistema de key embutido (key: chiquitazzqxkey1345)
-- Funciona no Delta e KRNL

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

local correctKey = "chiquitazzqxkey1345"
local keyEntered = false

-- Sistema de Key UI
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
            keyEntered = true
            ScreenGui:Destroy()
            -- Aqui chama a função para iniciar o painel
            createModMenu()
        else
            InfoLabel.Text = "❌ Key incorreta! Verifique no TTK."
        end
    end)

    GetKeyButton.MouseButton1Click:Connect(function()
        setclipboard(correctKey)
        InfoLabel.Text = "✅ Key copiada! Vá até o TTK e cole."
    end)
end

-- O painel completo (aqui vai todo seu código do menu RGB com Aimbot, ESP e AimKill)
function createModMenu()
    local aimbotEnabled = false
    local espEnabled = false
    local aimkillEnabled = false
    local targetPart = "Head"
    local radius = 150

    -- Bolinha RGB fixa no centro da tela
    local circle = Drawing.new("Circle")
    circle.Visible = true
    circle.Radius = radius
    circle.Thickness = 2
    circle.NumSides = 100
    circle.Filled = false
    circle.Transparency = 1
    circle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

    -- Função para pegar o player mais próximo dentro do raio da bolinha
    local function getClosestPlayer()
        local closest, dist = nil, radius
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild(targetPart) then
                local pos, onScreen = Camera:WorldToViewportPoint(player.Character[targetPart].Position)
                if onScreen then
                    local mag = (Vector2.new(pos.X, pos.Y) - circle.Position).Magnitude
                    if mag < dist then
                        closest = player
                        dist = mag
                    end
                end
            end
        end
        return closest
    end

    -- ESP que continua após a morte
    local function applyESP(player)
        local function attachESP()
            task.wait(1)
            if player.Character and not player.Character:FindFirstChild("CHX_ESP") then
                local highlight = Instance.new("Highlight", player.Character)
                highlight.Name = "CHX_ESP"
                highlight.FillTransparency = 0.4
                highlight.OutlineTransparency = 0
                highlight.Adornee = player.Character

                coroutine.wrap(function()
                    while espEnabled and highlight.Parent do
                        local t = tick()
                        highlight.FillColor = Color3.fromHSV(t % 5 / 5, 1, 1)
                        highlight.OutlineColor = Color3.fromHSV((t + 1.5) % 5 / 5, 1, 1)
                        task.wait()
                    end
                    if highlight and highlight.Parent then highlight:Destroy() end
                end)()
            end
        end

        player.CharacterAdded:Connect(function() attachESP() end)
        if player.Character then attachESP() end
    end

    local function enableESP()
        for _, p in ipairs(Players:GetPlayers()) do
            if p ~= LocalPlayer then applyESP(p) end
        end
        Players.PlayerAdded:Connect(function(p)
            if p ~= LocalPlayer then applyESP(p) end
        end)
    end

    -- Painel RGB com borda animada
    local gui = Instance.new("ScreenGui", game.CoreGui)
    gui.Name = "ChiquitazzqxModMenu"

    local frame = Instance.new("Frame", gui)
    frame.Size = UDim2.new(0, 250, 0, 140)
    frame.Position = UDim2.new(0.3, 0, 0.3, 0)
    frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    frame.BorderSizePixel = 4
    frame.Active = true
    frame.Draggable = true

    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 10)

    local title = Instance.new("TextLabel", frame)
    title.Size = UDim2.new(1, 0, 0, 30)
    title.Text = "🌈 Chiquitazzqx Mod Menu v2.9"
    title.Font = Enum.Font.GothamBold
    title.TextSize = 14
    title.TextColor3 = Color3.new(1, 1, 1)
    title.BackgroundTransparency = 1

    local btnAimbot = Instance.new("TextButton", frame)
    btnAimbot.Position = UDim2.new(0, 10, 0, 40)
    btnAimbot.Size = UDim2.new(0, 230, 0, 25)
    btnAimbot.Text = "🎯 Aimbot: OFF"
    btnAimbot.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    btnAimbot.TextColor3 = Color3.new(1, 1, 1)
    btnAimbot.Font = Enum.Font.Gotham
    btnAimbot.TextSize = 13

    btnAimbot.MouseButton1Click:Connect(function()
        aimbotEnabled = not aimbotEnabled
        btnAimbot.Text = "🎯 Aimbot: " .. (aimbotEnabled and "ON" or "OFF")
    end)

    local btnESP = Instance.new("TextButton", frame)
    btnESP.Position = UDim2.new(0, 10, 0, 75)
    btnESP.Size = UDim2.new(0, 230, 0, 25)
    btnESP.Text = "👁️ ESP: OFF"
    btnESP.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    btnESP.TextColor3 = Color3.new(1, 1, 1)
    btnESP.Font = Enum.Font.Gotham
    btnESP.TextSize = 13

    btnESP.MouseButton1Click:Connect(function()
        espEnabled = not espEnabled
        btnESP.Text = "👁️ ESP: " .. (espEnabled and "ON" or "OFF")
        if espEnabled then enableESP() end
    end)

    local btnAimKill = Instance.new("TextButton", frame)
    btnAimKill.Position = UDim2.new(0, 10, 0, 110)
    btnAimKill.Size = UDim2.new(0, 230, 0, 25)
    btnAimKill.Text = "🔥 AimKill: OFF"
    btnAimKill.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    btnAimKill.TextColor3 = Color3.new(1, 1, 1)
    btnAimKill.Font = Enum.Font.Gotham
    btnAimKill.TextSize = 13

    btnAimKill.MouseButton1Click:Connect(function()
        aimkillEnabled = not aimkillEnabled
        btnAimKill.Text = "🔥 AimKill: " .. (aimkillEnabled and "ON" or "OFF")
    end)

    -- Função para atacar automaticamente (adicione seu código aqui)
    local function attackTarget()
        pcall(function()
            -- Aqui coloque seu código para disparar (evento remoto, clique, etc)
            -- Exemplo:
            -- game:GetService("ReplicatedStorage").ShootEvent:FireServer()
        end)
    end

    -- AimKill: segue e fica colado no inimigo, atacando automático
    local currentTarget = nil
    RunService.Heartbeat:Connect(function()
        if aimkillEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            -- Se não tem alvo ou o atual morreu, busca outro
            if not currentTarget or not currentTarget.Character or not currentTarget.Character:FindFirstChild(targetPart) then
                currentTarget = nil
                for _, player in ipairs(Players:GetPlayers()) do
                    if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild(targetPart) then
                        local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
                        if humanoid and humanoid.Health > 0 then
                            currentTarget = player
                            break
                        end
                    end
                end
            end

            if currentTarget and currentTarget.Character and currentTarget.Character:FindFirstChild(targetPart) then
                local humanoid = currentTarget.Character:FindFirstChildOfClass("Humanoid")
                if humanoid and humanoid.Health > 0 then
                    local headCFrame = currentTarget.Character[targetPart].CFrame
                    local offset = headCFrame.LookVector * -2
                    LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(headCFrame.Position + offset, headCFrame.Position)
                    attackTarget()
                else
                    currentTarget = nil
                end
            end
        end
    end)

    -- Loop principal (bolinha fixa e aimbot)
    RunService.RenderStepped:Connect(function()
        circle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

        if aimbotEnabled then
            local target = getClosestPlayer()
            if target and target.Character and target.Character:FindFirstChild(targetPart) then
                local pos = target.Character[targetPart].Position
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, pos)
            end
        end
    end)

    -- RGB animado para bolinha e painel
    coroutine.wrap(function()
        while true do
            local hsv = Color3.fromHSV(tick() % 5 / 5, 1, 1)
            circle.Color = hsv
            frame.BorderColor3 = hsv
            title.TextColor3 = hsv
            task.wait()
        end
    end)()
end

-- Inicia a keyUI
createKeyUI()
