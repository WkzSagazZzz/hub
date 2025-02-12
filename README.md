-- Configurações iniciais
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- Criar elementos principais
local ScreenGui = Instance.new("ScreenGui")
local Panel = Instance.new("Frame")
local MinimizeButton = Instance.new("TextButton")
local CloseButton = Instance.new("TextButton")
local TabContainer = Instance.new("Frame")
local ContentContainer = Instance.new("Frame")

-- Configuração do painel principal
Panel.Name = "Panel"
Panel.Size = UDim2.new(0, 400, 0, 350)
Panel.Position = UDim2.new(0.5, -200, 0.5, -175)
Panel.BackgroundColor3 = Color3.fromRGB(20, 20, 20) -- Fundo escuro
Panel.BorderSizePixel = 0
Panel.ClipsDescendants = true
Panel.Parent = ScreenGui

-- Borda arredondada
local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 8)
Corner.Parent = Panel

-- Botão de minimizar (+)
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Text = "+"
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.TextSize = 20
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(1, -35, 0, 5)
MinimizeButton.AutoButtonColor = false
MinimizeButton.Parent = Panel

local MinimizeCorner = Instance.new("UICorner")
MinimizeCorner.CornerRadius = UDim.new(0, 6)
MinimizeCorner.Parent = MinimizeButton

-- Botão de fechar (X)
CloseButton.Name = "CloseButton"
CloseButton.Text = "X"
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 20
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -70, 0, 5)
CloseButton.AutoButtonColor = false
CloseButton.Parent = Panel

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 6)
CloseCorner.Parent = CloseButton

-- Container das abas
TabContainer.Name = "TabContainer"
TabContainer.Size = UDim2.new(0, 100, 1, -40)
TabContainer.Position = UDim2.new(0, 0, 0, 40)
TabContainer.BackgroundColor3 = Color3.fromRGB(30, 30, 30) -- Fundo escuro para abas
TabContainer.BorderSizePixel = 0
TabContainer.Parent = Panel

-- Container de conteúdo
ContentContainer.Name = "ContentContainer"
ContentContainer.Size = UDim2.new(1, -100, 1, -40)
ContentContainer.Position = UDim2.new(0, 100, 0, 40)
ContentContainer.BackgroundColor3 = Color3.fromRGB(25, 25, 25) -- Fundo escuro para conteúdo
ContentContainer.BorderSizePixel = 0
ContentContainer.Parent = Panel

-- Função para criar abas
local function createTab(name, content, order)
    local TabButton = Instance.new("TextButton")
    TabButton.Name = name .. "Tab"
    TabButton.Text = name
    TabButton.Font = Enum.Font.Gotham
    TabButton.TextSize = 16
    TabButton.TextColor3 = Color3.fromRGB(200, 200, 200)
    TabButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    TabButton.Size = UDim2.new(1, 0, 0, 35)
    TabButton.Position = UDim2.new(0, 0, 0, 40 * (order - 1)) -- Espaçamento entre as abas
    TabButton.BorderSizePixel = 0
    TabButton.AutoButtonColor = false
    TabButton.Parent = TabContainer

    local TabCorner = Instance.new("UICorner")
    TabCorner.CornerRadius = UDim.new(0, 6)
    TabCorner.Parent = TabButton

    -- Efeito de hover
    TabButton.MouseEnter:Connect(function()
        TweenService:Create(TabButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(60, 60, 60)}):Play()
    end)

    TabButton.MouseLeave:Connect(function()
        if TabButton.Text ~= "Main" then
            TweenService:Create(TabButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
        end
    end)

    -- Conteúdo da aba
    local TabContent = Instance.new("Frame")
    TabContent.Name = name .. "Content"
    TabContent.Size = UDim2.new(1, 0, 1, 0)
    TabContent.BackgroundTransparency = 1
    TabContent.Visible = false
    TabContent.Parent = ContentContainer

    -- Adicionar conteúdo à aba
    if content then
        local Label = Instance.new("TextLabel")
        Label.Text = content
        Label.Font = Enum.Font.Gotham
        Label.TextSize = 20
        Label.TextColor3 = Color3.fromRGB(255, 255, 255)
        Label.Size = UDim2.new(1, -20, 1, -20)
        Label.Position = UDim2.new(0, 10, 0, 10)
        Label.BackgroundTransparency = 1
        Label.Parent = TabContent
    end

    -- Ativar aba ao clicar
    TabButton.MouseButton1Click:Connect(function()
        for _, tab in pairs(TabContainer:GetChildren()) do
            if tab:IsA("TextButton") then
                TweenService:Create(tab, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
            end
        end

        for _, content in pairs(ContentContainer:GetChildren()) do
            if content:IsA("Frame") then
                content.Visible = false
            end
        end

        TweenService:Create(TabButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(70, 70, 70)}):Play()
        TabContent.Visible = true
    end)

    return TabButton, TabContent
end

-- Criar abas com ordem específica
local mainTab, mainContent = createTab("Main", "Bem-vindo ao painel!", 1)
local farmTab, farmContent = createTab("Farm", "Configurações de Farm", 2)
local teleportTab, teleportContent = createTab("Teleport", "Locações de Teleporte", 3)
local creditsTab, creditsContent = createTab("Credits", "Credits - WkzSagaz", 4) -- Nova aba 'Credits'

-- Definir aba padrão
mainTab.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
mainContent.Visible = true

-- Funcionalidade de minimizar
local isMinimized = false
local minimizedSize = UDim2.new(0, 50, 0, 50)
local originalSize = Panel.Size
local originalPosition = Panel.Position

MinimizeButton.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized

    if isMinimized then
        -- Minimizar
        TweenService:Create(Panel, TweenInfo.new(0.3), {Size = minimizedSize}):Play()
        TweenService:Create(MinimizeButton, TweenInfo.new(0.3), {Position = UDim2.new(0.5, -15, 0.5, -15)}):Play()
        MinimizeButton.Text = ""
        CloseButton.Visible = false
        TabContainer.Visible = false
        ContentContainer.Visible = false
    else
        -- Maximizar
        TweenService:Create(Panel, TweenInfo.new(0.3), {Size = originalSize}):Play()
        TweenService:Create(MinimizeButton, TweenInfo.new(0.3), {Position = UDim2.new(1, -35, 0, 5)}):Play()
        MinimizeButton.Text = "+"
        CloseButton.Visible = true
        TabContainer.Visible = true
        ContentContainer.Visible = true
    end
end)

-- Fechar painel
CloseButton.MouseButton1Click:Connect(function()
    Panel.Visible = false
end)

-- Restaurar painel ao clicar no quadrado minimizado
Panel.InputBegan:Connect(function(input)
    if isMinimized and input.UserInputType == Enum.UserInputType.MouseButton1 then
        Panel.Visible = true
        isMinimized = false
        TweenService:Create(Panel, TweenInfo.new(0.3), {Size = originalSize}):Play()
        TweenService:Create(MinimizeButton, TweenInfo.new(0.3), {Position = UDim2.new(1, -35, 0, 5)}):Play()
        MinimizeButton.Text = "+"
        CloseButton.Visible = true
        TabContainer.Visible = true
        ContentContainer.Visible = true
    end
end)

-- Mover painel
local dragging = false
local dragStart = Vector2.new()
local startPos = Vector2.new()

Panel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = Panel.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

Panel.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
        local delta = input.Position - dragStart
        Panel.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

-- Responsividade
UserInputService:GetPropertyChangedSignal("TouchEnabled"):Connect(function()
    if UserInputService.TouchEnabled then
        Panel.Size = UDim2.new(0, 350, 0, 300)
        Panel.Position = UDim2.new(0.5, -175, 0.5, -150)
    else
        Panel.Size = UDim2.new(0, 400, 0, 350)
        Panel.Position = UDim2.new(0.5, -200, 0.5, -175)
    end
end)

-- Finalizar
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
