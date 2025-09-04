-- DP SCRIPTS - Versão Atualizada
local DP = {}
DP.GUI = {}
DP.Modules = {}
DP.InfMappingActive = false

-- Função para criar o painel principal
function DP:CreatePanel()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "DP_SCRIPTS"
    ScreenGui.Parent = game:GetService("CoreGui")

    local MainFrame = Instance.new("Frame")
    MainFrame.Size = UDim2.new(0,400,0,300)
    MainFrame.Position = UDim2.new(0.5,-200,0.5,-150)
    MainFrame.BackgroundColor3 = Color3.fromRGB(35,35,35)
    MainFrame.Parent = ScreenGui
    self.GUI.MainFrame = MainFrame

    -- Exemplo de botão de Começar/Parar Mapeamento
    local MapButton = Instance.new("TextButton")
    MapButton.Size = UDim2.new(0,120,0,40)
    MapButton.Position = UDim2.new(0,10,0,10)
    MapButton.Text = "Começar Mapeamento"
    MapButton.BackgroundColor3 = Color3.fromRGB(50,50,50)
    MapButton.TextColor3 = Color3.fromRGB(255,255,255)
    MapButton.Parent = MainFrame

    MapButton.MouseButton1Click:Connect(function()
        DP.InfMappingActive = not DP.InfMappingActive
        if DP.InfMappingActive then
            MapButton.Text = "Parar Mapeamento"
            DP:StartMapping()
        else
            MapButton.Text = "Começar Mapeamento"
            DP:StopMapping()
        end
    end)

    -- Botão Freeze INF
    local FreezeButton = Instance.new("TextButton")
    FreezeButton.Size = UDim2.new(0,80,0,30)
    FreezeButton.Position = UDim2.new(0,10,0,60)
    FreezeButton.Text = "Freeze INF"
    FreezeButton.Parent = MainFrame
    FreezeButton.MouseButton1Click:Connect(function()
        DP.Modules.FreezeINF = not DP.Modules.FreezeINF
    end)

    -- Botão Copiar INF
    local CopyButton = Instance.new("TextButton")
    CopyButton.Size = UDim2.new(0,80,0,30)
    CopyButton.Position = UDim2.new(0,100,0,60)
    CopyButton.Text = "Copiar INF"
    CopyButton.Parent = MainFrame
    CopyButton.MouseButton1Click:Connect(function()
        setclipboard(DP.Modules.CapturedData or "")
    end)
end

-- Função para iniciar o mapeamento
function DP:StartMapping()
    spawn(function()
        while DP.InfMappingActive do
            if not DP.Modules.FreezeINF then
                -- Exemplo: Scan de objetos fixos dos minigames
                local data = {}
                for i,v in pairs(workspace:GetDescendants()) do
                    if v:IsA("BasePart") then
                        table.insert(data, v.Name.." | "..tostring(v.CanCollide))
                    end
                end
                DP.Modules.CapturedData = table.concat(data,"\n")
            end
            wait(0.3) -- otimizado para reduzir travamento
        end
    end)
end

function DP:StopMapping()
    -- Para automaticamente ao mudar DP.InfMappingActive
end

-- Exemplos de módulos (Glass Bridge, Jump Rope, etc.)
function DP:InitModules()
    -- Aqui você adicionaria as funções automáticas dos minigames
    -- Placeholder para Glass Bridge, Jump Rope, Dalgona, Hidden Seek, Batatinha Frita 123
end

-- Inicializar
DP:CreatePanel()
DP:InitModules()
