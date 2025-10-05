# Image-Uploade-using-FromData-base64

```jsx

import { FaCamera, FaImage } from 'react-icons/fa';
import { ChevronRight } from 'lucide-react';
import { useRef, useState } from 'react';

const UploadProfilePage = () => {
    const [imagePreview, setImagePreview] = useState("https://cdn.pixabay.com/photo/2025/05/23/08/54/girl-9617241_1280.png"); // Default image
    const [imageFile, setImageFile] = useState(null); // Store the image file
    const fileInputRef = useRef(null);

    // Handle image selection from file input
    const handleImageChange = (e) => {
        const file = e.target.files[0];

        // Basic validation for file type and size
        if (file) {
            if (!file.type.startsWith('image/')) {
                alert('Please upload a valid image.');
                return;
            }
            if (file.size > 15 * 1024 * 1024) { // 15MB limit
                alert('File size should not exceed 15MB.');
                return;
            }

            const reader = new FileReader();
            reader.onloadend = () => {
                setImagePreview(reader.result); // Set the image preview after reading the file
            };
            reader.readAsDataURL(file);
            setImageFile(file); // Store the file
        }
    };

    // Handle file input from gallery or camera
    const handleFromGallery = () => {
        if (fileInputRef.current) fileInputRef.current.click();
    };

    return (
        <div className="">
            {/* Main Section */}
            <div className="flex-1 flex flex-col items-center justify-center text-center max-w-xl mx-auto">

                {/* Profile Image Circle */}
                <div className="w-28 h-28 rounded-2xl border-4 border-white bg-gray-200 der-dashed shadow-2xl overflow-hidden flex items-center justify-center mb-2 cursor-pointer"
                    onClick={handleFromGallery}>
                    <img
                        src={imagePreview}
                        alt="Profile"
                        className="object-cover w-full h-full"
                    />
                </div>



                {/* Hidden File Input */}
                <input
                    type="file"
                    accept="image/*"
                    ref={fileInputRef}
                    className="hidden"
                    onChange={handleImageChange}
                />


            </div>

        </div>
    );
};

export default UploadProfilePage;

```
#Img upload with fullimg preview

```jsx
import { FaPencilAlt, FaTimes } from 'react-icons/fa'
import { useEffect, useRef, useState } from 'react'

const SocietyImgUpload = () => {
  const [imagePreview, setImagePreview] = useState(
    'https://cdn.pixabay.com/photo/2025/05/23/08/54/girl-9617241_1280.png'
  ) // Default image
  const [imageFile, setImageFile] = useState(null) // Store the image file
  const fileInputRef = useRef(null)

  // Fullscreen preview state
  const [isPreviewOpen, setIsPreviewOpen] = useState(false)

  // Handle image selection from file input
  const handleImageChange = (e) => {
    const file = e.target.files?.[0]
    if (!file) return

    // Basic validation for file type and size
    if (!file.type.startsWith('image/')) {
      alert('Please upload a valid image.')
      return
    }
    if (file.size > 15 * 1024 * 1024) {
      // 15MB limit
      alert('File size should not exceed 15MB.')
      return
    }

    const reader = new FileReader()
    reader.onloadend = () => {
      setImagePreview(reader.result)
    }
    reader.readAsDataURL(file)
    setImageFile(file)
  }

  // Open file picker
  const handleFromGallery = () => {
    if (fileInputRef.current) fileInputRef.current.click()
  }

  // Open fullscreen preview
  const openPreview = () => {
    if (imagePreview) setIsPreviewOpen(true)
  }

  // Close fullscreen preview
  const closePreview = () => setIsPreviewOpen(false)

  // ESC to close
  useEffect(() => {
    if (!isPreviewOpen) return
    const onKeyDown = (e) => {
      if (e.key === 'Escape') closePreview()
    }
    window.addEventListener('keydown', onKeyDown)
    return () => window.removeEventListener('keydown', onKeyDown)
  }, [isPreviewOpen])

  return (
    <div className="">
      {/* Main Section */}
      <div className="flex-1 flex flex-col items-center justify-center text-center max-w-xl mx-auto">
        {/* Profile Image Box */}
        <div
          className="relative w-28 h-28 rounded-2xl border-4 border-white bg-gray-200 shadow-2xl overflow-hidden flex items-center justify-center mb-2 cursor-pointer"
          onClick={openPreview}
          role="button"
          aria-label="Open image preview"
          title="Click to preview"
        >
          <img
            src={imagePreview}
            alt="Profile"
            className="object-cover w-full h-full"
          />

          {/* Edit icon (opens file picker) */}
          <button
            type="button"
            onClick={(e) => {
              e.stopPropagation() // prevent triggering preview
              handleFromGallery()
            }}
            className="absolute bottom-1 right-1 p-2 rounded-full bg-black/60 hover:bg-black/70 transition"
            aria-label="Change image"
            title="Change image"
          >
            <FaPencilAlt className="text-white" size={12} />
          </button>
        </div>

        {/* Hidden File Input */}
        <input
          type="file"
          accept="image/*"
          ref={fileInputRef}
          className="hidden"
          onChange={handleImageChange}
        />
      </div>

      {/* Fullscreen Preview Overlay */}
      {isPreviewOpen && (
        <div
          className="fixed inset-0 z-[9999] bg-black/90 backdrop-blur-sm flex items-center justify-center p-4"
          onClick={closePreview}
          aria-modal="true"
          role="dialog"
        >
          {/* Close button */}
          <button
            onClick={(e) => {
              e.stopPropagation()
              closePreview()
            }}
            className="absolute top-4 right-4 p-2 rounded-full bg-white/10 hover:bg-white/20 transition"
            aria-label="Close preview"
            title="Close"
          >
            <FaTimes className="text-white" size={18} />
          </button>

          {/* Image container (clicking image won't close) */}
          <div
            className="max-w-[95vw] max-h-[90vh]"
            onClick={(e) => e.stopPropagation()}
          >
            <img
              src={imagePreview}
              alt="Full preview"
              className="max-w-[95vw] max-h-[90vh] object-contain rounded-lg shadow-2xl"
            />
          </div>
        </div>
      )}
    </div>
  )
}

export default SocietyImgUpload

