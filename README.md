-- خدمات Roblox المستخدمة local StarterGui = game:GetService("StarterGui") local Players = game:GetService("Players") local TeleportService = game:GetService("TeleportService") local RunService = game:GetService("RunService") local UserInputService = game:GetService("UserInputService")

-- معرف المكان المستهدف للنقل التلقائي local PLACE_ID = 2961583129

-- تفعيل النقل التلقائي local function onButtonClick(option) if option == "Yes" then local player = Players.LocalPlayer if game.PlaceId == PLACE_ID then return end TeleportService:Teleport(PLACE_ID, player) end end

-- عرض إشعار للمستخدم if game.PlaceId ~= PLACE_ID then local Bindable = Instance.new("BindableFunction") Bindable.OnInvoke = onButtonClick

StarterGui:SetCore("SendNotification", {
    Title = "تنبيه!",
    Text = "هل تريد الانتقال إلى المكان المحدد؟",
    Duration = 10,
    Button1 = "نعم",
    Button2 = "لا",
    Callback = Bindable,
})
return

end

-- تحميل مكتبة Fluent لإنشاء الواجهة الرسومية local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))() local Executor = identifyexecutor and identifyexecutor() or "غير معروف"

local Window = Fluent:CreateWindow({ Title = "تم التطوير بواسطة Jaimz - الإصدار 1.1", SubTitle = "المشغل: " .. Executor, TabWidth = 160, Size = UDim2.fromOffset(580, 460), Acrylic = true, Theme = "Dark", MinimizeKey = Enum.KeyCode.LeftControl })

local Tabs = { Main = Window:AddTab({ Title = "الرئيسية", Icon = "" }), Settings = Window:AddTab({ Title = "الإعدادات", Icon = "settings" }) } Window:SelectTab(1)

-- إضافة زر نسخ رابط الديسكورد Tabs.Main:AddButton({ Title = "انضم إلى الديسكورد للإبلاغ عن الأخطاء", Callback = function() setclipboard("https://discord.gg/cuRJbSdrtZ") end })

-- ميزة رؤية اللاعبين من خلال الجدران (ESP) local function enableESP() for _, player in ipairs(Players:GetPlayers()) do if player ~= Players.LocalPlayer and player.Character then for _, part in ipairs(player.Character:GetChildren()) do if part:IsA("BasePart") then part.Material = Enum.Material.ForceField part.Color = Color3.new(1, 0, 0) end end end end end

-- ميزة القفز التلقائي عند الحاجة UserInputService.InputBegan:Connect(function(input) if input.KeyCode == Enum.KeyCode.Space then Players.LocalPlayer.Character.Humanoid.Jump = true end end)

-- ميزة زيادة السرعة عند الضغط على زر معين local speedEnabled = false Tabs.Main:AddToggle("SpeedToggle", { Title = "تمكين زيادة السرعة", Description = "اضغط على R لزيادة السرعة", Default = false, Callback = function(state) speedEnabled = state end })

UserInputService.InputBegan:Connect(function(input) if input.KeyCode == Enum.KeyCode.R and speedEnabled then Players.LocalPlayer.Character.Humanoid.WalkSpeed = 50 else Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16 end end)

-- ميزة منع الطرد بسبب الخمول local function preventAFK() game:GetService("Players").LocalPlayer.Idled:Connect(function() game:GetService("VirtualUser"):Button2Down(Vector2.new(0, 0), workspace.CurrentCamera.CFrame) wait(1) game:GetService("VirtualUser"):Button2Up(Vector2.new(0, 0), workspace.CurrentCamera.CFrame) end) end preventAFK()

-- ميزة إعادة الدخول تلقائيًا عند الطرد local function autoRejoin() game:GetService("CoreGui").RobloxPromptGui.promptOverlay.ChildAdded:Connect(function(child) if child.Name == "ErrorPrompt" then TeleportService:Teleport(game.PlaceId, Players.LocalPlayer) end end) end autoRejoin()

-- ميزة إرسال رسائل تلقائية في الشات Tabs.Main:AddButton({ Title = "إرسال رسالة تلقائية", Callback = function() game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer("أنا أستخدم سكربت خاص!", "All") end })

