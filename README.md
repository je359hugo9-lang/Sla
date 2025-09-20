-- Script Lua para o Mod Menu flutuante "UNIVERSAL PREMIUM"
local player = game.Players.LocalPlayer
local mouse = player:GetMouse()

-- Criando a interface (GUI)
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui
screenGui.Name = "UniversalPremiumMenu"

-- Função para criar botões
local function createButton(name, position, parent, func)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Text = name
    button.Size = UDim2.new(0, 150, 0, 50)
    button.Position = position
    button.Parent = parent
    button.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 18
    button.MouseButton1Click:Connect(func)
    return button
end

-- Funções do Mod Menu
local ESPEnabled = false
local SpeedBoostEnabled = false
local NoClipEnabled = false
local GravityEnabled = false
local SavedPosition = nil
local IsMenuOpen = true

-- Função para ativar/desativar o ESP
local function toggleESP()
    ESPEnabled = not ESPEnabled
    for _, v in pairs(game.Players:GetPlayers()) do
        if ESPEnabled then
            local billboard = Instance.new("BillboardGui")
            billboard.Adornee = v.Character.Head
            billboard.Parent = v.Character.Head
            billboard.Size = UDim2.new(0, 100, 0, 50)
            billboard.StudsOffset = Vector3.new(0, 2, 0)
            local label = Instance.new("TextLabel")
            label.Parent = billboard
            label.Text = v.Name
            label.BackgroundTransparency = 1
        else
            for _, obj in pairs(v.Character.Head:GetChildren()) do
                if obj:IsA("BillboardGui") then
                    obj:Destroy()
                end
            end
        end
    end
end

-- Função para ativar/desativar Speed Booster
local function toggleSpeedBooster()
    SpeedBoostEnabled = not SpeedBoostEnabled
    local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        if SpeedBoostEnabled then
            humanoid.WalkSpeed = humanoid.WalkSpeed * 1.5 -- Aumenta a velocidade em 50%
        else
            humanoid.WalkSpeed = humanoid.WalkSpeed / 1.5 -- Restaura a velocidade normal
        end
    end
end

-- Função para ativar/desativar NoClip
local function toggleNoClip()
    NoClipEnabled = not NoClipEnabled
    local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.PlatformStand = NoClipEnabled
        if NoClipEnabled then
            player.Character:FindFirstChild("HumanoidRootPart").Anchored = true
        else
            player.Character:FindFirstChild("HumanoidRootPart").Anchored = false
        end
    end
end

-- Função para ativar/desativar Gravity
local function toggleGravity()
    GravityEnabled = not GravityEnabled
    if GravityEnabled then
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(10000, 10000, 10000)
        bodyVelocity.Velocity = Vector3.new(0, 10, 0)
        bodyVelocity.Parent = player.Character.HumanoidRootPart
    else
        for _, child in pairs(player.Character.HumanoidRootPart:GetChildren()) do
            if child:IsA("BodyVelocity") then
                child:Destroy()
            end
        end
    end
end

-- Função para salvar a posição do jogador
local function savePosition()
    SavedPosition = player.Character.HumanoidRootPart.Position
    local part = Instance.new("Part")
    part.Shape = Enum.PartType.Ball
    part.Size = Vector3.new(1, 1, 1)
    part.Position = SavedPosition
    part.Anchored = true
    part.CanCollide = false
    part.BrickColor = BrickColor.new("Bright red")
    part.Parent = game.Workspace
end

-- Função para teleportar para a posição salva
local function teleportToSavedPosition()
    if SavedPosition then
        player.Character.HumanoidRootPart.CFrame = CFrame.new(SavedPosition)
    else
        print("Posição não salva!")
    end
end

-- Função para abrir/fechar o menu
local function toggleMenu()
    IsMenuOpen = not IsMenuOpen
    if IsMenuOpen then
        screenGui.Enabled = true
    else
        screenGui.Enabled = false
    end
end

-- Criando o painel principal do Mod Menu
local menuFrame = Instance.new("Frame")
menuFrame.Size = UDim2.new(0, 200, 0, 300)
menuFrame.Position = UDim2.new(0, 50, 0, 50)
menuFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
menuFrame.BackgroundTransparency = 0.5
menuFrame.Parent = screenGui

-- Criando o botão para fechar/abrir o menu
local toggleButton = createButton("Toggle Menu", UDim2.new(0, 0, 0, 0), menuFrame, toggleMenu)

-- Criando as funções dentro do menu
createButton("ESP NAME", UDim2.new(0, 0, 0, 60), menuFrame, toggleESP)
createButton("SPEED BOOSTER", UDim2.new(0, 0, 0, 120), menuFrame, toggleSpeedBooster)
createButton("NOCLIP", UDim2.new(0, 0, 0, 180), menuFrame, toggleNoClip)
createButton("GRAVITY", UDim2.new(0, 0, 0, 240), menuFrame, toggleGravity)
createButton("SAVE POSITION", UDim2.new(0, 0, 0, 300), menuFrame, savePosition)
createButton("TELEPORT", UDim2.new(0, 0, 0, 360), menuFrame, teleportToSavedPosition)

-- Permitindo mover o painel pela tela
local dragging = false
local dragStart = nil
local startPos = nil

menuFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = menuFrame.Position
    end
end)

menuFrame.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        menuFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

menuFrame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

-- Abrindo o menu automaticamente
screenGui.Enabled = IsMenuOpen-- Script Lua para o Mod Menu flutuante "UNIVERSAL PREMIUM"
local player = game.Players.LocalPlayer
local mouse = player:GetMouse()

-- Criando a interface (GUI)
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui
screenGui.Name = "UniversalPremiumMenu"

-- Função para criar botões
local function createButton(name, position, parent, func)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Text = name
    button.Size = UDim2.new(0, 150, 0, 50)
    button.Position = position
    button.Parent = parent
    button.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 18
    button.MouseButton1Click:Connect(func)
    return button
end

-- Funções do Mod Menu
local ESPEnabled = false
local SpeedBoostEnabled = false
local NoClipEnabled = false
local GravityEnabled = false
local SavedPosition = nil
local IsMenuOpen = true

-- Função para ativar/desativar o ESP
local function toggleESP()
    ESPEnabled = not ESPEnabled
    for _, v in pairs(game.Players:GetPlayers()) do
        if ESPEnabled then
            local billboard = Instance.new("BillboardGui")
            billboard.Adornee = v.Character.Head
            billboard.Parent = v.Character.Head
            billboard.Size = UDim2.new(0, 100, 0, 50)
            billboard.StudsOffset = Vector3.new(0, 2, 0)
            local label = Instance.new("TextLabel")
            label.Parent = billboard
            label.Text = v.Name
            label.BackgroundTransparency = 1
        else
            for _, obj in pairs(v.Character.Head:GetChildren()) do
                if obj:IsA("BillboardGui") then
                    obj:Destroy()
                end
            end
        end
    end
end

-- Função para ativar/desativar Speed Booster
local function toggleSpeedBooster()
    SpeedBoostEnabled = not SpeedBoostEnabled
    local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        if SpeedBoostEnabled then
            humanoid.WalkSpeed = humanoid.WalkSpeed * 1.5 -- Aumenta a velocidade em 50%
        else
            humanoid.WalkSpeed = humanoid.WalkSpeed / 1.5 -- Restaura a velocidade normal
        end
    end
end

-- Função para ativar/desativar NoClip
local function toggleNoClip()
    NoClipEnabled = not NoClipEnabled
    local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.PlatformStand = NoClipEnabled
        if NoClipEnabled then
            player.Character:FindFirstChild("HumanoidRootPart").Anchored = true
        else
            player.Character:FindFirstChild("HumanoidRootPart").Anchored = false
        end
    end
end

-- Função para ativar/desativar Gravity
local function toggleGravity()
    GravityEnabled = not GravityEnabled
    if GravityEnabled then
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(10000, 10000, 10000)
        bodyVelocity.Velocity = Vector3.new(0, 10, 0)
        bodyVelocity.Parent = player.Character.HumanoidRootPart
    else
        for _, child in pairs(player.Character.HumanoidRootPart:GetChildren()) do
            if child:IsA("BodyVelocity") then
                child:Destroy()
            end
        end
    end
end

-- Função para salvar a posição do jogador
local function savePosition()
    SavedPosition = player.Character.HumanoidRootPart.Position
    local part = Instance.new("Part")
    part.Shape = Enum.PartType.Ball
    part.Size = Vector3.new(1, 1, 1)
    part.Position = SavedPosition
    part.Anchored = true
    part.CanCollide = false
    part.BrickColor = BrickColor.new("Bright red")
    part.Parent = game.Workspace
end

-- Função para teleportar para a posição salva
local function teleportToSavedPosition()
    if SavedPosition then
        player.Character.HumanoidRootPart.CFrame = CFrame.new(SavedPosition)
    else
        print("Posição não salva!")
    end
end

-- Função para abrir/fechar o menu
local function toggleMenu()
    IsMenuOpen = not IsMenuOpen
    if IsMenuOpen then
        screenGui.Enabled = true
    else
        screenGui.Enabled = false
    end
end

-- Criando o painel principal do Mod Menu
local menuFrame = Instance.new("Frame")
menuFrame.Size = UDim2.new(0, 200, 0, 300)
menuFrame.Position = UDim2.new(0, 50, 0, 50)
menuFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
menuFrame.BackgroundTransparency = 0.5
menuFrame.Parent = screenGui

-- Criando o botão para fechar/abrir o menu
local toggleButton = createButton("Toggle Menu", UDim2.new(0, 0, 0, 0), menuFrame, toggleMenu)

-- Criando as funções dentro do menu
createButton("ESP NAME", UDim2.new(0, 0, 0, 60), menuFrame, toggleESP)
createButton("SPEED BOOSTER", UDim2.new(0, 0, 0, 120), menuFrame, toggleSpeedBooster)
createButton("NOCLIP", UDim2.new(0, 0, 0, 180), menuFrame, toggleNoClip)
createButton("GRAVITY", UDim2.new(0, 0, 0, 240), menuFrame, toggleGravity)
createButton("SAVE POSITION", UDim2.new(0, 0, 0, 300), menuFrame, savePosition)
createButton("TELEPORT", UDim2.new(0, 0, 0, 360), menuFrame, teleportToSavedPosition)

-- Permitindo mover o painel pela tela
local dragging = false
local dragStart = nil
local startPos = nil

menuFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = menuFrame.Position
    end
end)

menuFrame.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        menuFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

menuFrame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

-- Abrindo o menu automaticamente
screenGui.Enabled = IsMenuOpen
