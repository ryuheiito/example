from PIL import Image
import os

def crop_and_enlarge_images(folder_path, output_folder, x, y, width, height, enlargement_factor):
    # 出力フォルダが存在しない場合は作成する
    if not os.path.exists(output_folder):
        os.makedirs(output_folder)

    # フォルダ内のすべてのpngファイルを処理する
    for filename in os.listdir(folder_path):
        if filename.endswith(".png"):
            # 画像のパス
            image_path = os.path.join(folder_path, filename)

            # 画像を開く
            image = Image.open(image_path)

            # 切り抜き範囲を指定して画像を切り抜く
            cropped_image = image.crop((x, y, x + width, y + height))

            # 拡大する
            enlarged_image = cropped_image.resize((width * enlargement_factor, height * enlargement_factor))

            # 切り抜いた後に拡大した画像を保存するパス
            output_path = os.path.join(output_folder, filename)

            # 切り抜いた後に拡大した画像を保存
            enlarged_image.save(output_path)

            print(f"Cropped and enlarged image saved as: {output_path}")

# 使用例
folder_path = "output"
output_folder = "output/conv"
x = 720  # 切り抜き位置のx座標（左上隅）
y = 550  # 切り抜き位置のy座標（左上隅）
width = 650  # 切り抜き幅（ピクセル）
height = 500  # 切り抜き高さ（ピクセル）
enlargement_factor = 2# 拡大倍率

crop_and_enlarge_images(folder_path, output_folder, x, y, width, height, enlargement_factor)
