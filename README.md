local P, RS, VIM = game:GetService("Players"), game:GetService("RunService"), game:GetService("VirtualInputManager")
local L, Cam = P.LocalPlayer, workspace.CurrentCamera

-- [[ ULTRA ANTI-KICK & BYPASS PRO GLOBAL ]]
pcall(function()
    local mt = getrawmetatable(game)
    local oldNC, oldIdx, oldNewIdx = mt.__namecall, mt.__index, mt.__newindex
    setreadonly(mt, false)
    
    mt.__index = newcclosure(function(t, k)
        if not checkcaller() and t == L then
            if k == "WalkSpeed" then return 16 end
            if k == "JumpPower" then return 50 end
            if k == "JumpHeight" then return 7.2 end
        end
        return oldIdx(t, k)
    end)
    
    mt.__newindex = newcclosure(function(t, k, v)
        if not checkcaller() and t == L then
            if k == "WalkSpeed" or k == "JumpPower" or k == "JumpHeight" then return nil end
        end
        return oldNewIdx(t, k, v)
    end)
    
    mt.__namecall = newcclosure(function(self, ...)
        local m = getnamecallmethod()
        if not checkcaller() then
            if m:find("Kick") or m:find("kick") then return nil end
            if m == "FireServer" then
                local n = self.Name:lower()
                if n:find("report") or n:find("log") or n:find("cheat") or n:find("anticheat") or n:find("ban") or n:find("detection") then
                    return nil
                end
            end
        end
        return oldNC(self, ...)
    end)
    setreadonly(mt, true)
end)

_G.K = {A = false, F = 150, SF = false, AF = false, EB = false, ES = false, EH = false, EN = false, EI = false, EU = false, EY = false, HT = false, NC = false, S = 16, J = 50, BH = false, WB = false, FLY = false, TIdx = 1, LastFire = 0, HB = false, INF = false, TARG = false, TPB = false, X = false, RP = false, SP = false, MHB = false}
local ThemeColor = Color3.fromRGB(205, 0, 0)

local CurrentLang = "TR"
local LangPack = {
    TR = {
        Title = "ŞİFREYİ GİR", Place = "Buraya yazın...", Link = "Key almak için Telegram kanalımıza katılın\nBağlantı: t.me/kabusc", Wrong = "YANLIŞ ŞİFRE!", Copied = "Telegram Linki Kopyalandı!", 
        Ev = "EV", Aim = "⌖ AIM ⌖", Esp = "⚯ ESP ⚯", Move = "웃 HAREKET 웃", Other = "⚒ DIGER 🛠", Target = "HEDEF SEC", TargetPlr = "HEDEF: ", GoTo = "YANINA GIT!",
        SA = "SILENT AIM", SF = "SHOW FOV", F150 = "FOV: 150", F300 = "FOV: 300", F500 = "FOV: 500",
        EN = "ISIM ESP", EH = "CAN ESP", EY = "HESAP YASI", EI = "ID ESP", EU = "ULKE ESP", ES = "ISKELET ESP", EB = "BOX ESP", HT = "TAKIM GIZLE",
        X = "OLD SCHOOL ANIM", SP = "SPINBOT",
        S150 = "HIZ: 150", J150 = "ZIPLAMA: 150", FLY = "FLY (UCMA)", BH = "BUNNYHOP", WB = "WALLBANG", NC = "NOCLIP", HB = "GIZLI HITBOX", INF = "SONSUZ MERMI", RP = "SERI ATES (RAPID)", ColorTitle = "RENKLER 🎨", MHB = "🛡️ MINI HITBOX (GOD)"
    },
    EN = {
        Title = "ENTER PASSWORD", Place = "Type here...", Link = "Join our Telegram channel to get the key\nLink: t.me/kabusc", Wrong = "WRONG PASSWORD!", Copied = "Telegram Link Copied!", 
        Ev = "HOME", Aim = "⌖ AIM ⌖", Esp = "⚯ ESP ⚯", Move = "웃 MOVEMENT 웃", Other = "⚒ OTHER 🛠", Target = "SELECT TARGET", TargetPlr = "TARGET: ", GoTo = "TELEPORT!",
        SA = "SILENT AIM", SF = "SHOW FOV", F150 = "FOV: 150", F300 = "FOV: 300", F500 = "FOV: 500",
        EN = "NAME ESP", EH = "HEALTH ESP", EY = "ACCOUNT AGE", EI = "ID ESP", EU = "COUNTRY ESP", ES = "SKELETON ESP", EB = "BOX ESP", HT = "HIDE TEAM",
        X = "OLD SCHOOL ANIM", SP = "SPINBOT",
        S150 = "SPEED: 150", J150 = "JUMP: 150", FLY = "FLY", BH = "BUNNYHOP", WB = "WALLBANG", NC = "NOCLIP", HB = "SILENT HITBOX", INF = "INFINITE AMMO", RP = "RAPID FIRE", ColorTitle = "COLORS 🎨", MHB = "🛡️ MINI HITBOX (GOD)"
    }
}

-- [[ FOV CIRCLE ]]
local FG = Instance.new("ScreenGui", L.PlayerGui); FG.Name = "KFov"; FG.ResetOnSpawn = false
local FC = Instance.new("Frame", FG); FC.BackgroundTransparency = 1; FC.Visible = false; FC.Size = UDim2.new(0, 300, 0, 300); FC.AnchorPoint = Vector2.new(0.5, 0.5); FC.Position = UDim2.new(0.5, 0, 0.5, 0)
local ST = Instance.new("UIStroke", FC); ST.Thickness = 1.5; ST.Color = Color3.new(1,1,1); Instance.new("UICorner", FC).CornerRadius = UDim.new(1,0)

local function GetLocale(plr)
    local loc = ".."
    pcall(function() local l = plr.LocaleId:lower() if l:find("tr") then loc = "TR 🇹🇷" else loc = l:sub(1,2):upper() end end)
    return loc
end

-- [[ UI SYSTEM ]]
local G = Instance.new("ScreenGui", L.PlayerGui); G.Name = "KArchive"; G.ResetOnSpawn = false
local M = Instance.new("Frame", G); M.Size = UDim2.new(0, 360, 0, 295); M.AnchorPoint = Vector2.new(0.5, 0.5); M.Position = UDim2.new(0.5, 0, 0.5, 0); M.BackgroundColor3 = Color3.fromRGB(15, 15, 15); M.Active = true; M.Draggable = true; Instance.new("UICorner", M)

local Side = Instance.new("Frame", M); Side.Size = UDim2.new(0, 100, 1, 0); Side.BackgroundColor3 = Color3.fromRGB(22, 22, 22); Instance.new("UICorner", Side); Side.Visible = false
local Cont = Instance.new("Frame", M); Cont.Position = UDim2.new(0.31, 5, 0, 10); Cont.Size = UDim2.new(0.69, -10, 1, -20); Cont.BackgroundTransparency = 1; Cont.Visible = false
local Sig = Instance.new("TextLabel", Side); Sig.Size = UDim2.new(1, 0, 0, 40); Sig.Position = UDim2.new(0, 0, 1, -45); Sig.BackgroundTransparency = 1; Sig.Text = "KABUS\nCHEATS"; Sig.TextColor3 = Color3.new(1, 1, 1); Sig.Font = "GothamBold"; Sig.TextSize = 10

-- [[ SHIFRE PANELI VE LINK ]]
local Auth = Instance.new("Frame", M); Auth.Size = UDim2.new(1, 0, 1, 0); Auth.BackgroundTransparency = 1
local AuthTitle = Instance.new("TextLabel", Auth); AuthTitle.Size = UDim2.new(1, 0, 0, 40); AuthTitle.Position = UDim2.new(0, 0, 0.1, 0); AuthTitle.BackgroundTransparency = 1; AuthTitle.Text = "ŞİFREYİ GİR"; AuthTitle.TextColor3 = Color3.new(1, 1, 1); AuthTitle.Font = "GothamBold"; AuthTitle.TextSize = 16
local AuthBox = Instance.new("TextBox", Auth); AuthBox.Size = UDim2.new(0, 200, 0, 32); AuthBox.Position = UDim2.new(0.5, -100, 0.3, 0); AuthBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30); AuthBox.TextColor3 = Color3.new(1, 1, 1); AuthBox.Font = "GothamBold"; AuthBox.TextSize = 11; AuthBox.Text = ""; AuthBox.PlaceholderText = "Buraya yazın..."
local AuthStroke = Instance.new("UIStroke", AuthBox); AuthStroke.Color = Color3.fromRGB(60, 60, 60); AuthStroke.Thickness = 1.5

local AuthLink = Instance.new("TextButton", Auth); AuthLink.Size = UDim2.new(1, 0, 0, 35); AuthLink.Position = UDim2.new(0, 0, 0.48, 0); AuthLink.BackgroundTransparency = 1; AuthLink.Text = "Key almak için Telegram kanalımıza katılın\nBağlantı: t.me/kabusc"; AuthLink.TextColor3 = Color3.fromRGB(0, 170, 255); AuthLink.Font = "GothamBold"; AuthLink.TextSize = 9

local LangBtn = Instance.new("TextButton", Auth); LangBtn.Size = UDim2.new(0, 140, 0, 28); LangBtn.Position = UDim2.new(0.5, -70, 0.67, 0); LangBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 35); LangBtn.Text = "DİLLER / LANGUAGES 🌐"; LangBtn.TextColor3 = Color3.new(1,1,1); LangBtn.Font = "GothamBold"; LangBtn.TextSize = 9; Instance.new("UICorner", LangBtn)
local LangMenu = Instance.new("Frame", Auth); LangMenu.Size = UDim2.new(0, 140, 0, 110); LangMenu.Position = UDim2.new(0.5, -70, 0.78, 0); LangMenu.BackgroundColor3 = Color3.fromRGB(25, 25, 25); LangMenu.Visible = false; LangMenu.ZIndex = 5; Instance.new("UICorner", LangMenu); local LMS = Instance.new("UIStroke", LangMenu); LMS.Color = Color3.fromRGB(70, 70, 70)

local function ShowNotif(text)
    local bG = Instance.new("ScreenGui", L.PlayerGui); bG.Name = "KNotif"
    local bF = Instance.new("Frame", bG); bF.Size = UDim2.new(0,250,0,50); bF.Position = UDim2.new(0.5,-125,0,-100); bF.BackgroundColor3 = Color3.fromRGB(30,30,30); Instance.new("UICorner", bF); Instance.new("UIStroke", bF).Color = Color3.new(1,1,1)
    local bL = Instance.new("TextLabel", bF); bL.Size = UDim2.new(1,0,1,0); bL.BackgroundTransparency = 1; bL.Text = text; bL.TextColor3 = Color3.new(1,1,1); bL.Font = "GothamBold"; bL.TextSize = 10
    bF:TweenPosition(UDim2.new(0.5,-125,0.05,0), "Out", "Back", 0.5)
    task.spawn(function() task.wait(2.5); bF:TweenPosition(UDim2.new(0.5,-125,0,-100), "In", "Quad", 0.5); task.wait(0.6); bG:Destroy() end)
end

AuthLink.MouseButton1Click:Connect(function()
    setclipboard("https://t.me/kabusc")
    ShowNotif(LangPack[CurrentLang].Copied)
end)

local function SetLanguage(lang)
    CurrentLang = lang or "TR"
    local p = LangPack[CurrentLang]
    AuthTitle.Text = p.Title
    AuthBox.PlaceholderText = p.Place
    AuthLink.Text = p.Link
    LangMenu.Visible = false
end

LangBtn.MouseButton1Click:Connect(function() LangMenu.Visible = not LangMenu.Visible end)

local function AddLangOpt(txt, code, y)
    local b = Instance.new("TextButton", LangMenu); b.Size = UDim2.new(1, -10, 0, 22); b.Position = UDim2.new(0, 5, 0, y); b.Text = txt; b.BackgroundColor3 = Color3.fromRGB(40, 40, 40); b.TextColor3 = Color3.new(1,1,1); b.Font = "GothamBold"; b.TextSize = 9; b.ZIndex = 6; Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function() SetLanguage(code) end)
end
AddLangOpt("Türkçe", "TR", 5); AddLangOpt("English", "EN", 30)

local Tabs = {}
local TabButtons = {}
local function CreateTab(codeKey, y)
    local b = Instance.new("TextButton", Side); b.Size = UDim2.new(1,-10,0,32); b.Position = UDim2.new(0,5,0,y); b.BackgroundColor3 = Color3.fromRGB(35,35,35); b.TextColor3 = Color3.new(1,1,1); b.Font = "GothamBold"; b.TextSize = 9; Instance.new("UICorner", b)
    local f = Instance.new("ScrollingFrame", Cont); f.Size = UDim2.new(1,0,1,0); f.BackgroundTransparency = 1; f.Visible = false; f.ScrollBarThickness = 0; 
    b.MouseButton1Click:Connect(function() 
        b.BackgroundColor3 = Color3.fromRGB(20,20,20); task.wait(0.1); b.BackgroundColor3 = Color3.fromRGB(35,35,35)
        for _,v in pairs(Tabs) do v.Visible = false end f.Visible = true 
    end); table.insert(Tabs, f); table.insert(TabButtons, {btn = b, key = codeKey}); return f
end

local hT = CreateTab("Ev", 5); local aT = CreateTab("Aim", 42); local eT = CreateTab("Esp", 79); local mT = CreateTab("Move", 116); local xT = CreateTab("Other", 153)

local hL = Instance.new("TextLabel", hT); hL.Size = UDim2.new(1,0,0,40); hL.Position = UDim2.new(0,0,0,5); hL.Text = "KABUS CHEATS SUNAR 🇹🇷"; hL.TextColor3 = Color3.new(1,1,1); hL.BackgroundTransparency = 1; hL.Font = "GothamBold"; hL.TextSize = 12
local tBtn = Instance.new("TextButton", hT); tBtn.Size = UDim2.new(1,-20,0,30); tBtn.Position = UDim2.new(0,10,0,50); tBtn.Text = "t.me/kabusc"; tBtn.BackgroundColor3 = Color3.fromRGB(0, 136, 204); tBtn.TextColor3 = Color3.new(1,1,1); tBtn.Font = "GothamBold"; tBtn.TextSize = 11; Instance.new("UICorner", tBtn)

tBtn.MouseButton1Click:Connect(function() 
    setclipboard("https://t.me/kabusc")
    ShowNotif(LangPack[CurrentLang].Copied)
end)

local btnList = {}
local K 

local function UpdateColors()
    for _, i in pairs(btnList) do
        local act = (_G.K[i.f] == (i.val or true))
        if i.f == "S" then act = (_G.K.S == 150) end
        if i.f == "J" then act = (_G.K.J == 150) end
        i.b.BackgroundColor3 = act and ThemeColor or Color3.fromRGB(45,45,45)
    end
    if K then K.BackgroundColor3 = ThemeColor end
end

local ColorMainBtn = Instance.new("TextButton", hT); ColorMainBtn.Size = UDim2.new(1, -20, 0, 30); ColorMainBtn.Position = UDim2.new(0, 10, 0, 90); ColorMainBtn.Text = "RENKLER 🎨"; ColorMainBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 35); ColorMainBtn.TextColor3 = Color3.new(1, 1, 1); ColorMainBtn.Font = "GothamBold"; ColorMainBtn.TextSize = 10; Instance.new("UICorner", ColorMainBtn)
local ColorScroll = Instance.new("ScrollingFrame", hT); ColorScroll.Size = UDim2.new(1, -20, 0, 110); ColorScroll.Position = UDim2.new(0, 10, 0, 125); ColorScroll.BackgroundColor3 = Color3.fromRGB(22, 22, 22); ColorScroll.Visible = false; ColorScroll.ScrollBarThickness = 3; ColorScroll.CanvasSize = UDim2.new(0, 0, 0, 245); Instance.new("UICorner", ColorScroll); local CSS = Instance.new("UIStroke", ColorScroll); CSS.Color = Color3.fromRGB(50, 50, 50)

ColorMainBtn.MouseButton1Click:Connect(function() ColorScroll.Visible = not ColorScroll.Visible end)

local ColorsTable = {
    {N = "BEYAZ", C = Color3.fromRGB(255, 255, 255)},
    {N = "SİYAH", C = Color3.fromRGB(0, 0, 0)},
    {N = "KIRMIZI", C = Color3.fromRGB(205, 0, 0)},
    {N = "YEŞİL", C = Color3.fromRGB(0, 185, 0)},
    {N = "MAVİ", C = Color3.fromRGB(0, 120, 255)},
    {N = "SARI", C = Color3.fromRGB(255, 210, 0)},
    {N = "LACİVERT", C = Color3.fromRGB(0, 35, 120)},
    {N = "PEMBE", C = Color3.fromRGB(255, 105, 180)},
    {N = "TURUNCU", C = Color3.fromRGB(255, 130, 0)}
}

for idx, data in ipairs(ColorsTable) do
    local cb = Instance.new("TextButton", ColorScroll); cb.Size = UDim2.new(1, -10, 0, 24); cb.Position = UDim2.new(0, 5, 0, (idx-1)*27 + 3); cb.Text = data.N; cb.BackgroundColor3 = Color3.fromRGB(40, 40, 40); cb.TextColor3 = data.C; cb.Font = "GothamBold"; cb.TextSize = 9; Instance.new("UICorner", cb)
    cb.MouseButton1Click:Connect(function() ThemeColor = data.C; UpdateColors(); ColorScroll.Visible = false end)
end

local function Add(tab, txtCode, f, y, val)
    local b = Instance.new("TextButton", tab); b.Size = UDim2.new(1,0,0,28); b.Position = UDim2.new(0,0,0,y); b.BackgroundColor3 = Color3.fromRGB(45,45,45); b.TextColor3 = Color3.new(1,1,1); b.Font = "GothamBold"; b.TextSize = 9; Instance.new("UICorner", b)
    table.insert(btnList, {b = b, f = f, val = val, code = txtCode})
    b.MouseButton1Click:Connect(function() 
        b.BackgroundColor3 = Color3.fromRGB(20,20,20); task.wait(0.05)
        if f == "S" then _G.K.S = (_G.K.S == 150) and 16 or 150
        elseif f == "J" then _G.K.J = (_G.K.J == 150) and 50 or 150
        elseif val ~= nil then _G.K[f] = val
        else _G.K[f] = not _G.K[f] end
        UpdateColors()
    end); return b
end

-- TABS LOAD DYNAMIC LABELS
local bSA = Add(aT, "SA", "A", 0); local bSF = Add(aT, "SF", "SF", 33); local bF1 = Add(aT, "F150", "F", 66, 150); local bF2 = Add(aT, "F300", "F", 99, 300); local bF3 = Add(aT, "F500", "F", 132, 500)
local bEN = Add(eT, "EN", "EN", 0); local bEH = Add(eT, "EH", "EH", 33); local bEY = Add(eT, "EY", "EY", 66); local bEI = Add(eT, "EI", "EI", 99); local bEU = Add(eT, "EU", "EU", 132); local bES = Add(eT, "ES", "ES", 165); local bEB = Add(eT, "EB", "EB", 198); local bHT = Add(eT, "HT", "HT", 231)
local bX = Add(mT, "X", "X", 0); bX.MouseButton1Click:Connect(function() pcall(function() local a = Instance.new("Animation"); a.AnimationId = "rbxassetid://531982821"; L.Character.Humanoid:LoadAnimation(a):Play() end) end)
local bSP = Add(mT, "SP", "SP", 33)
local bS1 = Add(xT, "S150", "S", 0); local bJ1 = Add(xT, "J150", "J", 33); local bFLY = Add(xT, "FLY", "FLY", 66); local bBH = Add(xT, "BH", "BH", 99); local bWB = Add(xT, "WB", "WB", 132); local bNC = Add(xT, "NC", "NC", 165); local bHB = Add(xT, "HB", "HB", 198); local bINF = Add(xT, "INF", "INF", 231)
local sel = Add(xT, "Target", "TARG", 264, false); local tp = Add(xT, "GoTo", "TPB", 297, false); local bRP = Add(xT, "RP", "RP", 330)
local bMHB = Add(xT, "MHB", "MHB", 363) -- NANO HITBOX MOD

-- TOGGLE BUTTON
K = Instance.new("TextButton", G); K.Size = UDim2.new(0,35,0,35); K.Position = UDim2.new(0.9,0,0.1,0); K.Text = "K"; K.BackgroundColor3 = ThemeColor; K.TextColor3 = Color3.new(1,1,1); Instance.new("UICorner", K); K.Active = true; K.Draggable = true; K.Visible = false
K.MouseButton1Down:Connect(function() K:TweenSize(UDim2.new(0, 30, 0, 30), "Out", "Quad", 0.1, true) end)
K.MouseButton1Up:Connect(function() K:TweenSize(UDim2.new(0, 35, 0, 35), "Out", "Back", 0.2, true); M.Visible = not M.Visible end)

sel.MouseButton1Click:Connect(function() local p = P:GetPlayers(); _G.K.TIdx = (_G.K.TIdx % #p) + 1; sel.Text = LangPack[CurrentLang].TargetPlr .. p[_G.K.TIdx].Name end)
tp.MouseButton1Click:Connect(function() local t = P:GetPlayers()[_G.K.TIdx]; if t and t.Character then L.Character:SetPrimaryPartCFrame(t.Character.HumanoidRootPart.CFrame * CFrame.new(0,3,0)) end end)

-- [[ SHIFRE KONTROLU ]]
AuthBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        if AuthBox.Text == "KABUS" then
            Auth:Destroy()
            Side.Visible = true; Cont.Visible = true; hT.Visible = true; K.Visible = true
            local p = LangPack[CurrentLang]
            for _, tb in pairs(TabButtons) do tb.btn.Text = p[tb.key] end
            for _, btnObj in pairs(btnList) do btnObj.b.Text = p[btnObj.code] or btnObj.b.Text end
            sel.Text = p.Target; tp.Text = p.GoTo; ColorMainBtn.Text = p.ColorTitle or "RENKLER 🎨"
            UpdateColors()
        else
            AuthBox.Text = ""; AuthBox.PlaceholderText = LangPack[CurrentLang].Wrong
            task.delay(1.5, function() AuthBox.PlaceholderText = LangPack[CurrentLang].Place end)
        end
    end
end)

-- [[ ANA DÖNGÜ ]]
RS.RenderStepped:Connect(function()
    if not K.Visible then return end
    FC.Visible = _G.K.SF
    if _G.K.SF then FC.Size = UDim2.new(0, _G.K.F*2, 0, _G.K.F*2) end
    
    local c = L.Character; local h = c and c:FindFirstChild("Humanoid"); local hrp = c and c:FindFirstChild("HumanoidRootPart")
    
    if h and hrp then
        h.WalkSpeed = _G.K.S; h.JumpPower = _G.K.J; h.UseJumpPower = true
        if _G.K.BH and h.FloorMaterial ~= Enum.Material.Air then h.Jump = true end
        if _G.K.FLY then hrp.Velocity = (Cam.CFrame.LookVector * (h.MoveDirection.Magnitude > 0 and _G.K.S or 0)) + Vector3.new(0,0.5,0) end
        if _G.K.NC then for _, v in pairs(c:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end end
        if _G.K.SP then hrp.CFrame = hrp.CFrame * CFrame.Angles(0, 0.87, 0) end
        
        -- [[ DYNAMIC MINI HITBOX ENGINE ]]
        for _, part in pairs(c:GetChildren()) do
            if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                if _G.K.MHB then
                    if not part:FindFirstChild("OrigSize") then
                        local orig = Instance.new("Vector3Value", part); orig.Name = "OrigSize"; orig.Value = part.Size
                    end
                    part.Size = Vector3.new(0.01, 0.01, 0.01) -- Hitboxı imkansız boyuta indir
                else
                    local orig = part:FindFirstChild("OrigSize")
                    if orig then
                        part.Size = orig.Value; orig:Destroy() -- Kapatıldığında eski boyutuna döndür
                    end
                end
            end
        end
    end
    
    local target = nil; local shortestDistance = math.huge; local players = P:GetPlayers()
    
    for i = 1, #players do
        local v = players[i]
        if v ~= L and v.Character then
            local char = v.Character; local head = char:FindFirstChild("Head")
            if head and char:FindFirstChild("Humanoid") and char.Humanoid.Health > 0 then
                local isT = (v.Team == L.Team and v.Team ~= nil)
                
                local hl = char:FindFirstChild("KVisual")
                if (_G.K.EB or _G.K.ES) and not (_G.K.HT and isT) then
                    if not hl then hl = Instance.new("Highlight", char); hl.Name = "KVisual"; hl.FillTransparency = 0.4; hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop end
                    local parts = Cam:GetPartsObscuringTarget({head.Position}, {c, char})
                    hl.FillColor = (#parts > 0) and Color3.new(1,0,0) or Color3.new(0,1,0)
                elseif hl then hl:Destroy() end

                local tag = head:FindFirstChild("KTag")
                if (_G.K.EN or _G.K.EH or _G.K.EI or _G.K.EU or _G.K.EY) and not (_G.K.HT and isT) then
                    if not tag then tag = Instance.new("BillboardGui", head); tag.Name = "KTag"; tag.Size = UDim2.new(0,120,0,70); tag.AlwaysOnTop = true; local tl = Instance.new("TextLabel", tag); tl.Size = UDim2.new(1,0,1,0); tl.BackgroundTransparency = 1; tl.TextColor3 = Color3.new(1,1,1); tl.Font = "GothamBold"; tl.TextSize = 8 end
                    local content = ""
                    if _G.K.EN then content = content .. v.Name .. " " end
                    if _G.K.EU then content = content .. "[" .. GetLocale(v) .. "] " end
                    if _G.K.EH then content = content .. "\nHP: " .. math.floor(char.Humanoid.Health) end
                    if _G.K.EY then content = content .. "\nYAS: " .. v.AccountAge .. " Gun" end
                    tag.TextLabel.Text = content
                elseif tag then tag:Destroy() end

                if _G.K.A and not isT then
                    local p, vis = Cam:WorldToViewportPoint(head.Position)
                    if vis or _G.K.WB then
                        local d = (Vector2.new(p.X, p.Y) - Vector2.new(Cam.ViewportSize.X/2, Cam.ViewportSize.Y/2)).Magnitude
                        if d < (_G.K.TARG and 99999 or _G.K.F) and d < shortestDistance then shortestDistance = d; target = v end
                    end
                end
            end
        end
    end

    if target and target.Character and target.Character:FindFirstChild("Head") then 
        Cam.CFrame = CFrame.new(Cam.CFrame.Position, target.Character.Head.Position)
        if _G.K.AF and tick() - _G.K.LastFire > 0.18 then _G.K.LastFire = tick(); VIM:SendMouseButtonEvent(0,0,0,true,game,0); task.wait(0.01); VIM:SendMouseButtonEvent(0,0,0,false,game,0) end
    end
end)

-- [[ ARSENAL & EVRENSEL SİLAH DÖNGÜSÜ ]]
local WeaponCache = {}
task.spawn(function()
    while task.wait(0.4) do
        if _G.K.INF or _G.K.RP then 
            pcall(function()
                if #WeaponCache == 0 then
                    for _, v in pairs(getgc(true)) do 
                        if type(v) == "table" and (rawget(v, "Ammo") or rawget(v, "m_ammo") or rawget(v, "Clip") or rawget(v, "Mag") or rawget(v, "FireRate")) then
                            table.insert(WeaponCache, v)
                        elseif type(v) == "function" and debug and debug.getupvalues then
                            for _, up in pairs(debug.getupvalues(v)) do
                                if type(up) == "table" and (rawget(up, "Ammo") or rawget(up, "CurrentAmmo") or rawget(up, "MaxAmmo")) then table.insert(WeaponCache, up) end
                            end
                        end
                    end
                end
                for i = 1, #WeaponCache do
                    local v = WeaponCache[i]
                    if v then
                        if _G.K.INF then
                            if rawget(v, "Ammo") then v.Ammo = 999999 end
                            if rawget(v, "m_ammo") then v.m_ammo = 999999 end
                            if rawget(v, "Clip") then v.Clip = 999999 end
                            if rawget(v, "Mag") then v.Mag = 999999 end
                            if rawget(v, "CurrentAmmo") then v.CurrentAmmo = 999999 end
                        end
                        if _G.K.RP then
                            if rawget(v, "FireRate") then v.FireRate = 0 end
                            if rawget(v, "FireCooldown") then v.FireCooldown = 0 end
                        end
                    end
                end
            end)
        else
            if #WeaponCache > 0 then table.clear(WeaponCache) end
        end 
    end
end)
            
