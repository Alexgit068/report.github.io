import React, { useState } from 'react';
import { Calendar, User, Tag, Package, Send, AlertCircle } from 'lucide-react';

const App = () => {
  const [formData, setFormData] = useState({
    saleDate: '',
    manager: '',
    segment: '',
    product: ''
  });
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [submitSuccess, setSubmitSuccess] = useState(false);
  const [submitError, setSubmitError] = useState('');

  const managers = ['Алексей Иванов', 'Мария Петрова', 'Дмитрий Сидоров', 'Елена Козлова', 'Сергей Морозов'];
  const segments = ['B2B', 'B2C', 'Корпоративный', 'Розница', 'Онлайн'];
  const products = ['Продукт A', 'Продукт B', 'Продукт C', 'Услуга X', 'Услуга Y'];

  const handleInputChange = (field, value) => {
    setFormData(prev => ({ ...prev, [field]: value }));
    // Clear any previous errors when user starts typing
    if (submitError) setSubmitError('');
  };

  const validateForm = () => {
    const { saleDate, manager, segment, product } = formData;
    if (!saleDate || !manager || !segment || !product) {
      return 'Пожалуйста, заполните все поля';
    }
    if (new Date(saleDate) > new Date()) {
      return 'Дата продажи не может быть в будущем';
    }
    return null;
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    const validationError = validateForm();
    if (validationError) {
      setSubmitError(validationError);
      return;
    }

    setIsSubmitting(true);
    setSubmitError('');
    
    // Simulate API call to Google Sheets
    try {
      await new Promise(resolve => setTimeout(resolve, 1500));
      setSubmitSuccess(true);
      // Reset form after successful submission
      setFormData({
        saleDate: '',
        manager: '',
        segment: '',
        product: ''
      });
      // Hide success message after 3 seconds
      setTimeout(() => setSubmitSuccess(false), 3000);
    } catch (error) {
      setSubmitError('Ошибка при отправке данных. Попробуйте позже.');
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 flex items-center justify-center p-4">
      <div className="w-full max-w-md">
        {/* Telegram-like header */}
        <div className="bg-blue-500 text-white rounded-t-2xl p-4 flex items-center justify-between">
          <div className="flex items-center space-x-3">
            <div className="w-10 h-10 bg-white rounded-full flex items-center justify-center">
              <Package className="text-blue-500 w-6 h-6" />
            </div>
            <div>
              <h1 className="font-bold text-lg">Отчет о продажах</h1>
              <p className="text-blue-100 text-sm">Сбор данных для анализа</p>
            </div>
          </div>
        </div>

        {/* Form container */}
        <div className="bg-white rounded-b-2xl shadow-xl overflow-hidden">
          <form onSubmit={handleSubmit} className="p-6 space-y-6">
            {/* Sale Date */}
            <div className="space-y-2">
              <label className="flex items-center space-x-2 text-gray-700 font-medium">
                <Calendar className="w-4 h-4 text-blue-500" />
                <span>Дата продажи</span>
              </label>
              <input
                type="date"
                value={formData.saleDate}
                onChange={(e) => handleInputChange('saleDate', e.target.value)}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none transition-all"
                max={new Date().toISOString().split('T')[0]}
              />
            </div>

            {/* Manager */}
            <div className="space-y-2">
              <label className="flex items-center space-x-2 text-gray-700 font-medium">
                <User className="w-4 h-4 text-blue-500" />
                <span>Менеджер</span>
              </label>
              <select
                value={formData.manager}
                onChange={(e) => handleInputChange('manager', e.target.value)}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none transition-all bg-white"
              >
                <option value="">Выберите менеджера</option>
                {managers.map((manager, index) => (
                  <option key={index} value={manager}>{manager}</option>
                ))}
              </select>
            </div>

            {/* Segment */}
            <div className="space-y-2">
              <label className="flex items-center space-x-2 text-gray-700 font-medium">
                <Tag className="w-4 h-4 text-blue-500" />
                <span>Сегмент</span>
              </label>
              <select
                value={formData.segment}
                onChange={(e) => handleInputChange('segment', e.target.value)}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none transition-all bg-white"
              >
                <option value="">Выберите сегмент</option>
                {segments.map((segment, index) => (
                  <option key={index} value={segment}>{segment}</option>
                ))}
              </select>
            </div>

            {/* Product */}
            <div className="space-y-2">
              <label className="flex items-center space-x-2 text-gray-700 font-medium">
                <Package className="w-4 h-4 text-blue-500" />
                <span>Товар/Услуга</span>
              </label>
              <select
                value={formData.product}
                onChange={(e) => handleInputChange('product', e.target.value)}
                className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none transition-all bg-white"
              >
                <option value="">Выберите товар/услугу</option>
                {products.map((product, index) => (
                  <option key={index} value={product}>{product}</option>
                ))}
              </select>
            </div>

            {/* Submit Button */}
            <button
              type="submit"
              disabled={isSubmitting}
              className="w-full bg-blue-500 hover:bg-blue-600 text-white font-bold py-3 px-4 rounded-lg transition-all duration-200 flex items-center justify-center space-x-2 disabled:opacity-50 disabled:cursor-not-allowed"
            >
              {isSubmitting ? (
                <>
                  <div className="w-5 h-5 border-2 border-white border-t-transparent rounded-full animate-spin"></div>
                  <span>Отправка...</span>
                </>
              ) : (
                <>
                  <Send className="w-5 h-5" />
                  <span>Отправить отчет</span>
                </>
              )}
            </button>

            {/* Error Message */}
            {submitError && (
              <div className="flex items-center space-x-2 p-3 bg-red-50 text-red-700 rounded-lg border border-red-200">
                <AlertCircle className="w-5 h-5 flex-shrink-0" />
                <span className="text-sm">{submitError}</span>
              </div>
            )}

            {/* Success Message */}
            {submitSuccess && (
              <div className="flex items-center space-x-2 p-3 bg-green-50 text-green-700 rounded-lg border border-green-200">
                <div className="w-5 h-5 bg-green-500 rounded-full flex items-center justify-center">
                  <svg className="w-3 h-3 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M5 13l4 4L19 7"></path>
                  </svg>
                </div>
                <span className="text-sm">Отчет успешно отправлен в Google Таблицу!</span>
              </div>
            )}
          </form>

          {/* Footer note */}
          <div className="px-6 py-4 bg-gray-50 border-t border-gray-100">
            <p className="text-xs text-gray-500 text-center">
              Данные будут автоматически добавлены в Google Таблицу для дальнейшего анализа
            </p>
          </div>
        </div>
      </div>
    </div>
  );
};

export default App;
