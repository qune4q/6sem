https://colab.research.google.com/drive/1A4wcZpfyV4gCtdYvFmWv06b0Okf6KQnE?usp=sharing
<br> import cv2

vid_capture = cv2.VideoCapture(0)
if (vid_capture.isOpened() == False):
  print("Ошибка открытия видеофайла")
# Чтение fps и количества кадров
else:
  # Получить информацию о частоте кадров
  # Можно заменить 5 на CAP_PROP_FPS, это перечисления
  fps = vid_capture.get(5)
  print('Фреймов в секунду: ', fps,'FPS')
  # Получить количество кадров
  # Можно заменить 7 на CAP_PROP_FRAME_COUNT, это перечисления
  frame_count = vid_capture.get(7)
  print('Частота кадров: ', frame_count)
  print('\n-----------------------------\nДля завершения нажмите "q" или Esc...')

file_count = 0
while (vid_capture.isOpened()):
    # Метод vid_capture.read() возвращают кортеж, первым элементом является логическое значение
    # а вторым кадр
    ret, frame = vid_capture.read()

    if ret == True:




        # Gray, blur, adaptive threshold
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        print("gray")
        cv2.imshow("gray", gray)
        # https://medium.com/analytics-vidhya/gaussian-blurring-with-python-and-opencv-ba8429eb879b
        blur = cv2.GaussianBlur(gray, (33, 33), 0)
        print("blur")
        cv2.imshow("blur", blur)
        thresh = cv2.threshold(blur, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)[1]
        print(cv2.THRESH_BINARY, cv2.THRESH_OTSU)
        print("thresh")
        cv2.imshow("thresh", thresh)

        # Morphological transformations https://docs.opencv.org/4.x/d9/d61/tutorial_py_morphological_ops.html
        kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5))
        print("kernel")
        cv2.imshow("kernel", kernel)
        opening = cv2.morphologyEx(thresh, cv2.MORPH_OPEN, kernel)
        print("opening")
        cv2.imshow("opening", opening)
        # Find contours https://learnopencv.com/contour-detection-using-opencv-python-c/
        # CHAIN_APPROX_SIMPLE
        # Алгоритм сжимает горизонтальные, вертикальные и диагональные сегменты вдоль контура и оставляет только их конечные точки.
        # Это означает, что любая из точек вдоль прямых путей будет отклонена, и у нас останутся только конечные точки.
        cnts = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
        cnts = cnts[0] if len(cnts) == 2 else cnts[1]

        for c in cnts:
            # Find perimeter of contour
            perimeter = cv2.arcLength(c, True)
            # Perform contour approximation
            approx = cv2.approxPolyDP(c, 0.04 * perimeter, True)
            # print(approx)
            # We assume that if the contour has more than a certain
            # number of verticies, we can make the assumption
            # that the contour shape is a circle
            if len(approx) > 0:

                # Obtain bounding rectangle to get measurements
                x, y, w, h = cv2.boundingRect(c)

                # Find measurements
                diameter = w
                radius = w / 2

                # Find centroid
                M = cv2.moments(c)

                if M["m00"] != 0:
                    cX = int(M["m10"] / M["m00"])
                    cY = int(M["m01"] / M["m00"])
                # print(cX,cY)
                # Draw the contour and center of the shape on the image
                # cv2.rectangle(image,(x,y),(x+w,y+h),(0,255,0),4)
                cv2.drawContours(frame, [c], 0, (36, 255, 12), 1)
                # cv2.circle(image, (cX, cY), 15, (320, 159, 22), -1)

                # Draw line and diameter information
                # cv2.line(image, (x, y + int(h/2)), (x + w, y + int(h/2)), (156, 188, 24), 3)
                # cv2.putText(image, "Diameter: {}".format(diameter), (cX - 150, cY - 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (156, 188, 24), 3)

        cv2.imshow("frame" ,frame)
        cv2.imwrite('thresh.png', thresh)
        # cv2_imshow(thresh)
        cv2.imwrite('opening.png', opening)
        # cv2_imshow(opening)
        file_count += 1
        print('Кадр {0:04d}'.format(file_count))
        # writefile = 'Resources/Image_sequence/is42_{0:04d}.jpg'.format(file_count)
        # cv2.imwrite(writefile, frame)
        # 20 в миллисекундах, попробуйте увеличить значение, скажем, 50 и
        # понаблюдайте за изменениями в показе
        key = cv2.waitKey(20)

        thresh = cv2.threshold(blur, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)[1]
        print(cv2.THRESH_BINARY, cv2.THRESH_OTSU)
        print("thresh")

        cnts = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
        cnts = cnts[0] if len(cnts) == 2 else cnts[1]
        i = 0


        frame1 = frame + 1
        before = frame
        after = frame1
        filled_after = after.copy()
        for c in cnts:
            # Find perimeter of contour
            perimeter = cv2.arcLength(c, True)
            # Perform contour approximation
            approx = cv2.approxPolyDP(c, 0.04 * perimeter, True)
            # print(approx)
            # We assume that if the contour has more than a certain
            # number of verticies, we can make the assumption
            # that the contour shape is a circle
            if len(approx) > 0:
                # Python program to explain cv2.putText() method

                # Reading an image in default mode

                if M["m00"] != 0:
                    cX = int(M["m10"] / M["m00"])
                    cY = int(M["m01"] / M["m00"])
                    # print(cX,cY)
                    # Draw the contour and center of the shape on the image
                    # cv2.rectangle(image,(x,y),(x+w,y+h),(0,255,0),4)
                cv2.drawContours(frame, [c], 0, (36, 255, 12), 1)
                # cv2.circle(image, (cX, cY), 15, (320, 159, 22), -1)

                # Draw line and diameter information
                # cv2.line(image, (x, y + int(h/2)), (x + w, y + int(h/2)), (156, 188, 24), 3)
                # cv2.putText(image, "Diameter: {}".format(diameter), (cX - 150, cY - 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (156, 188, 24), 3)
                # font
                font = cv2.FONT_HERSHEY_SIMPLEX

                # org
                org = (cX, cY)

                # fontScale
                fontScale = 1

                # Blue color in BGR
                color = (255, 0, 0)

                # Line thickness of 2 px
                thickness = 2

                # Using cv2.putText() method
                image = cv2.putText(frame, str(i), org, font, fontScale, color, thickness, cv2.LINE_AA)
                i = i + 1
                # Displaying the image

                # Obtain bounding rectangle to get measurements
                x, y, w, h = cv2.boundingRect(c)

                # Find measurements
                diameter = w
                radius = w / 2

                # Find centroid
                M = cv2.moments(c)

        cv2.imshow("frame1", frame)
        cv2.imwrite('image.png', frame)

        cv2.imwrite('thresh.png', thresh)
        # cv2_imshow(thresh)
        cv2.imwrite('opening.png', opening)
        # cv2_imshow(opening)
        cv2.imshow("fr1", filled_after)
        for c in cnts:
            # Find perimeter of contour
            perimeter = cv2.arcLength(c, True)
            # Perform contour approximation
            approx = cv2.approxPolyDP(c, 0.04 * perimeter, True)
            # print(approx)
            # We assume that if the contour has more than a certain
            # number of verticies, we can make the assumption
            # that the contour shape is a circle
            if len(approx) > 0:
                # Python program to explain cv2.putText() method

                # Reading an image in default mode

                if M["m00"] != 0:
                    cX = int(M["m10"] / M["m00"])
                    cY = int(M["m01"] / M["m00"])
                    # print(cX,cY)
                    # Draw the contour and center of the shape on the image
                    # cv2.rectangle(image,(x,y),(x+w,y+h),(0,255,0),4)
                cv2.drawContours(filled_after, [c], 0, (36, 255, 12), 1)
                # cv2.circle(image, (cX, cY), 15, (320, 159, 22), -1)

                # Draw line and diameter information
                # cv2.line(image, (x, y + int(h/2)), (x + w, y + int(h/2)), (156, 188, 24), 3)
                # cv2.putText(image, "Diameter: {}".format(diameter), (cX - 150, cY - 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (156, 188, 24), 3)
                # font
                font = cv2.FONT_HERSHEY_SIMPLEX

                # org
                org = (cX, cY)

                # fontScale
                fontScale = 1

                # Blue color in BGR
                color = (255, 0, 0)

                # Line thickness of 2 px
                thickness = 2

                # Using cv2.putText() method
                image = cv2.putText(filled_after, str(i), org, font, fontScale, color, thickness, cv2.LINE_AA)
                i = i + 1
                # Displaying the image

                # Obtain bounding rectangle to get measurements
                x, y, w, h = cv2.boundingRect(c)

                # Find measurements
                diameter = w
                radius = w / 2

                # Find centroid
                M = cv2.moments(c)
        if (key == ord('q')) or key == 27:
            break
    else:
        break
https://colab.research.google.com/drive/13oIcpWIFv6T67UMyBKfAwDc95XQQmggw?hl=ru
